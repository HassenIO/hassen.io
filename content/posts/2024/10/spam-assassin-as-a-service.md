---
title: "Spam Assassin as a Service"
date: 2024-10-22T08:01:15+01:00
draft: false
summary: Making SpamAssassin available through an API
tags: ["API", "docker"]
---

![Just Gmail](/2024/10/unsplash-gmail.jpg "Just Gmail")

Hello world!

In this post I'm sharing a fun project I had to deliver for a much larger project: A Spam detector available as an API.

## What are the requirements?

The span detector that I was tasked to create is part of a much larger system. Basically, the need is to test if an outbound email can be assymiled to a spam, before sending the email.

The #1 open source solution for spam detection is [Spam Assassin](https://spamassassin.apache.org/). The question then is to make Spam Assassin available as an API.

The overall architecture is like the following:

![System Architecture of SpamAssassin with FastAPI server](/2024/10/system-architecture.png "System Architecture of SpamAssassin with FastAPI server")

Basically, our user queries the Docker container that will forward the query to our FastAPI server. It will then send the content of the email to SpamAssassin to get the score of the email and a report. This will be sent to our original user in a nice JSON format.

Sounds good? Let's get started...

## The application

Let's start by the docker image. Starting with an Ubuntu image, we install the necessary tools, most importantly SpamAssassin and Python for server API.

```dockerfile
FROM ubuntu:22.04

# Prevent interactive prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    spamassassin \
    spamc \
    python3 \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

COPY requirements.txt .
RUN pip3 install --no-cache-dir -r requirements.txt

# Create app directory and cd into it
WORKDIR /app
COPY . .

# This updates SpamAssassin rules
RUN sa-update

RUN mkdir /var/run/spamd
CMD ["sh", "-c", "spamd --allow-tell --create-prefs --max-children 5 --helper-home-dir /var/lib/spamassassin -u debian-spamd & python3 app.py"]

EXPOSE 5000
```

> NOTA: This is a simple and not optimized docker image. Consider it for education purpose only, adapt it for production.

The rest is just basic configuration, nothing obscure under the sun, except for the `CMD` in which we start SpamAssassin daemon and the Python server.

Speaking of the Python server, I used FastAPI, with only two REST endpoints:

- `POST /check` to check if the email is a Spam
- `GET /up` as a health check

I'm using Pydantic to ensure typing, especially we are handling a JSON input, and returning a JSON too:

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
import subprocess
import tempfile
import os


app = FastAPI(title="SpamAssassin as a Service")


class EmailMessage(BaseModel):
    content: str
    threshold: float = 5.0  # Default spam score threshold


class SpamResponse(BaseModel):
    is_spam: bool
    spam_score: float
    report: str


@app.post("/check", response_model=SpamResponse)
async def check_spam(email: EmailMessage):
    try:
        # Create temporary file for email content
        with tempfile.NamedTemporaryFile(mode='w+', delete=False) as temp_file:
            temp_file.write(email.content)
            temp_file_path = temp_file.name

        # Run SpamAssassin check
        result = subprocess.run(
            ['spamc', '-c', temp_file_path],
            capture_output=True,
            text=True
        )

        # Get detailed report
        report = subprocess.run(
            ['spamc', '-R', temp_file_path],
            capture_output=True,
            text=True
        )

        # Clean up temporary file
        os.unlink(temp_file_path)

        # Parse spam score from report
        try:
            spam_score = float(result.stdout.strip())
        except ValueError:
            spam_score = 0.0

        return SpamResponse(
            is_spam=spam_score >= email.threshold,
            spam_score=spam_score,
            report=report.stdout
        )

    except Exception as e:
        raise HTTPException(status_code=500, detail=str(e))


@app.get("/up")
async def health_check():
    try:
        # Check if SpamAssassin daemon is running
        subprocess.run(['pgrep', 'spamd'], check=True)
        return {"status": "healthy"}
    except subprocess.CalledProcessError:
        raise HTTPException(status_code=503, detail="SpamAssassin daemon is not running")
```

The `POST /check` endpoints expects a JSON body with the following information:

- `content` which is the content of the email we want to send
- `threshold` which is a floating number that scores the "spaminess" of our message. Basically, SpamAssassin will give the message a score based on some criteria, for example:
  - 2.0 BODY: Contains 'click here'
  - 3.2 BODY: Excessive use of uppercase text
  - 2.7 MONEY: Mentions large sum of money
  - 4.5 SCAM: Typical prize scam phraseology
  - 2.1 FREEPRIZE: Contains 'free' and 'prize'
  - 2.1 CREDIT_CARD: Asks for credit card details

By default we set the `threshold` to 5 if not provided by the body, but it can be customized for every query. It means that if the score given by SpamAssassin reaches or exceeds the threshold, it is marked as spam.

In our server, the JSON output is like the following:

```json
{
  "is_spam": false,
  "spam_score": 0.3,
  "report": "* -0.0 BODY: Human generated mail text
   * 0.2 BODY: Contains business-related terms
   * 0.1 BODY: Contains formal greeting and signature

   SpamAssassin Score: 0.3 points
   Status: HAM (threshold 5.0)"
}
```

The nice thing is that SpamAssassin provides a report for its scoring. I really liked this because it can give explanations for a user on why his email is detected as a Spam.

To test, run this for example:

```sh
curl -X POST "http://localhost:5000/check" \
     -H "Content-Type: application/json" \
     -d '{"content": "Buy now! Limited time offer!", "threshold": 5.0}'
```

Finally, here is the minimal **requirements.txt**:

```text
fastapi>=0.68.0
uvicorn>=0.15.0
pydantic>=1.8.2
python-multipart>=0.0.5
```

Those packages will be installed in the Docker image on build.

## Take away

This was really fun because I learned what makes an email a Spam. It's a nice and cheap internal solution, but I can imagine that a scale service if Spam-detection-as-a-service is much more complex.

Also SpamAssassin is not bullet proof. But again, this is a nice first step for teams that send emails in big volumes and don't want to be marked as spams.

---

Cheers,

Hassen
