---
title: "CI/CD pipeline for Rails 8 with Github Actions and Kamal"
date: 2024-10-22T08:01:15+01:00
draft: false
summary: Creating a robust yet simple CI/CD pipeline for Rails 8
tags: ["API", "docker"]
---

![Pipes and cables on a wall](/2024/06/unsplash-pipes.jpg "Photo by Khara Woods on Unsplash")

Hello world!

Have a reliable CI/CD pipeline is life changing. In this post, I share my notes on how to create a robust, yet simple, CI/CD pipeline for Ruby on Rails 8, using Github Actions and Kamal for deployment.

## Our goal

Let's walk through the steps required to set up a continuous integration and continuous deployment (CI/CD) pipeline for a Ruby on Rails 8 application.

We will leverage Github Actions for automating our workflows and Kamal for seamless deployment.

By the end of this guide, you will have a clear understanding of how to create a robust and simple CI/CD pipeline that ensures your Rails application is tested, built, and deployed efficiently.

But remember, every system is different, so adapt to your use case if my solution doesn't fit your requirements. Use this guide as a v0.1 rather than just a Copy/Paste. In doubt, check Stack Overflow or ask an LLM.

## Why Github Actions?

If I have choice on the pipeline engine, I always choose Github Actions, mostly for the following reasons:

- Already integrate into Github (where 99% of my codebase is)
- Has a generous free tier
- Very nice paradigms and easy to get started with

Honestly, any CI/CD tool will do the job. So pick the one you like or the one your company requires to use. But if you don't have an opinion yet, starting your DevOps journey with Github Actions is not a bad decision.

But the real question is: Why use a CI/CD tool? After all, Kamal deployments can be done manually from your command line. So why bother with incorporating this into another system?

Simply put: **Humans make errors.**

By using automated tools, you ensure you'll always run the same procedure no matter what. With a simple `git push` rather than a `kamal deploy` command.

Also, if you're within a team, centralizing your deployment tools within your Git repository is a smart choice to avoid conflicts on deploys (plus a ton of other Ops scenarios that can make your day a nightmare).

_"Set it once, run forever"_ is what you should aim for, wether you're a team of 1 or 10. Wether you're an intern or the CTO.

Automating deployments is tasting freedom ®

## Why Kamal?

Even though there are a ton of other deployments alternatives, Kamal is now encapsulated by default with new Rails 8 projects. And this is not a bad decision: It is a robust tool, used by 37Signals so you can be confident it will be well maintained, and is very easy to use (when setup properly).

Plus, everything that configures Kamal is comitted into Git, which makes it a step forward GitOps approach (even though there are still some manuels steps like setting up a VPS and DNS stuff).

## Assumptions

I'm assuming that you already have a Rails 8 application, with Kamal installed on your machine, a Github account, and a VPS already started and configured to allow SSH.

> Otherwise this post will become too big, and I don't want to explicit basic setup steps that can quickly be outdated. Thank you.

## Setup Github Actions

Github Actions are YAML files located in the **.github/** folder of your project.

Create a file **.github/workflows/deploy.yml**

```yml
name: Deploy onto Production

on:
  push:
    branches:
      - master

permissions:
  contents: read
  packages: write
  id-token: write

jobs:
  deploy:
    if: github.event_name == 'push' && github.ref == 'refs/heads/master'
    runs-on: ubuntu-24.04
    env:
      RAILS_ENV: production
      BUNDLE_WITH: tools
      BUNDLE_WITHOUT: "development test"
      DOCKER_BUILDKIT: 1

steps:
  - name: Checkout code
    uses: actions/checkout@v4

  - name: Set up Ruby
    uses: ruby/setup-ruby@v1
    with:
      ruby-version: .ruby-version
      bundler-cache: true

  - name: Set up Docker Buildx
    uses: docker/setup-buildx-action@v3

  - name: Set up SSH
    run: |
      mkdir -p ~/.ssh
      echo "$SSH_PRIVATE_KEY" > ~/.ssh/id_rsa
      chmod 600 ~/.ssh/id_rsa
      eval $(ssh-agent -s)
      ssh-add ~/.ssh/id_rsa
      ssh-keyscan ww.xx.yy.zz >> ~/.ssh/known_hosts
    env:
      SSH_PRIVATE_KEY: ${{ secrets.SSH_PRIVATE_KEY }}

  - name: Deploy using Kamal
    run: bin/kamal deploy
    env:
      RAILS_MASTER_KEY: ${{ secrets.RAILS_MASTER_KEY }}
      KAMAL_REGISTRY_PASSWORD: ${{ secrets.KAMAL_REGISTRY_PASSWORD }}
      POSTGRES_PASSWORD: ${{ secrets.POSTGRES_PASSWORD }}
```

Let me explain:

This deployment runs when we push new changes to the master branch (`github.event_name == 'push' && github.ref == 'refs/heads/master'`) And the pipeline will run on Ubuntu 24.04 (latest, at this time of writing).

We set the global environment to `production` and set `DOCKER_BUILDKIT=1` to have verbose Docker builds (useful in case of debugging pipeline issues).

All of this was the pipeline setup phase. Let's now tackle the steps of the pipeline:

First we get the code (though `actions/checkout@v4`) and setup Ruby in it (`ruby/setup-ruby@v1`).

Make sure to have the file `.ruby-version` commited in your project. The pipeline will read the version in it to match your settings.

Then we setup Docker's advanced build capabilities, so our docker image gets built and available for Kamal.

Then comes the SSH config in the step called **Set up SSH**. Here please ensure the value of `ww.xx.yy.zz` matches the IP address of your server.

Finally we run the Kamal deploy command with the necessary environment variables. The two minimal variables are `RAILS_MASTER_KEY` and `KAMAL_REGISTRY_PASSWORD`. I'm also regerencing PostgreSQL variables, but it might not be necessary for you if you're using SQLite for example.

## Github Secrets

Note that the last 2 previous steps involve using the `${{ secrets.XYZ }}` syntax. Here we are referencing Github Secrets values. We need to set them before running the pipeline.

Under your repo settings, look for **Secrets and variables** then **Actions** then **New repository secret**.

![Github Actions Secrets](/2024/06/github-actions-secrets.png "Github Actions Secrets")

Then fill the appropriate values for the 4 environment variables in our example:

- `SSH_PRIVATE_KEY` Your server SSH private key
- `RAILS_MASTER_KEY` Your Rails master key
- `KAMAL_REGISTRY_PASSWORD` The Docker registry password
- `POSTGRES_PASSWORD` The database password (depends on your case)

## How it works

Now on, when you push new code on your master branch, the pipeline triggers which will grab your latest changes, it setups SSH for secure access, then Kamal is triggered to make the deployment.

Note that Kamal will build the docker image of your codebase within the pipeline, pushes it to the docker registry, SSH into your server, then pull the image to run the container.

Most of the deployment process is done by Kamal. Your pipeline setup is just here to prepare the work for Kamal.

This is the most time consuming step. That's why we have set `BUNDLE_WITHOUT="development test"` which makes the bundling faster using only necessary production gems.

## Take away

Even though it looks tedious, it is totally worth it. Setting up the CI/CD pipeline should be done carefully so the rest of your journey becomes much more enjoyable.

Hopefully you have seen the power of Kamal in this setup. If you decide to go with another CI/CD solution, most changes will be in the setup, but the `kamal deploy` step remains the same because that's where the deployment action occurs.

Kamal has introduced an abstraction that frees us from specific pipeline tooling.

Note that the suggested workflow only covers a simple deployment through a push to `master`. More professional CI/CD pipelines also includes testing, deployment to other environments, feature deployments, reporting,... etc. Please treat this manual as your starting point, and enjoy your journey into DevOps.

See you soon.

Hassen
