---
title: "Ruby on Rails is Dead!... Not So Fast, My Friend!"
date: 2023-10-10T21:58:00+01:00
draft: false
summary: Talking about Ruby on Rails, and how it shines now more than before.
tags: ["Ruby on Rails", "Software"]
---

![Rollercoaster](/2023/10/rollercoaster.jpg "Rollercoaster are more fun in Family")

Hello World!

Yesterday, I shared my enthusiasm about the new Rails 7.1 and all the talks made at [Rails World 2023](https://rubyonrails.org/world) with some developer friends, and I got the following statement:

> "But Ruby and Ruby on Rails are dead technologies. The world has long moved toward JS and React..."

...

BLASPHEMY!

I love Ruby, JS, Python and Go (I didn't try other languages, except maybe also PHP :P)

But Ruby is by far my favorite, and is far from dead. It's still giving me smile everytime I use it, as similar as it did 11 years ago when I discovered it. All while still being continuously updated. And speaking about the Ruby on Rails framework, it's probably the strongest framework out there, whether you're building an MVP or scaling your start-up (YES! Rails scales very well, ask Github, Shopify or Basecamp).

So, that's how I got inspired for this blog post, and I encourage you to read it if you have a doubt about the health of Ruby and Rails.

## Rails Rollercoaster

Let's begin with a brief history lesson, shall we? Ruby on Rails, lovingly known as "Rails," was born in 2005, and it took the web development world by storm! Just like that classic rollercoaster ride, Rails had its ups and downs, but it left an indelible mark on the industry.

"But wait," you might say, "I've heard that Rails is as outdated as a floppy disk!" Well, hold your horses (or in this case, your Rails)! Rails is still alive and kicking, and its popularity is far from dead. Many companies, including GitHub, Airbnb, and Shopify, still rely on Rails for their core systems.

Now, you might wonder how Rails stacks up against modern contenders like React, Next.js, or other JavaScript-based frameworks. It's like comparing apples and oranges, my friend! Rails is excellent for certain types of web applications, especially those that prioritize convention over configuration.

## Ruby on Rails vs. the Modern World

Let's take a quick detour and compare a Rails concept to something you might be more familiar with: JavaScript and React.

### 1. MVC Architecture

In the Rails world, we love our Model-View-Controller (MVC) architecture. It's like having a clear blueprint for your web app's structure. React has its own way of doing things, with components taking center stage. Both approaches aim for organization and maintainability, just with different flavors.

In Ruby on Rails:

```ruby
# This is a Ruby code snippet in an ActiveRecord model
class User < ApplicationRecord
  validates :name, presence: true
end

```

In React:

```jsx
// This is a JavaScript code snippet in a React component
function User(props) {
  if (!props.name) {
    throw new Error("Name is required!");
  }
  // ...
}
```

With very well defined conventions, there is no "big" debate about how code should be organized. No big organizational refactoring. Most of refactoring work is done to improve the business logic, not how the internals of your app are interractinbg.

Also, the MVC architecture helps even the junior to quickly get onboarded, which is a dream with more "open frameworks".

### 2. Convention Over Configuration

Rails is all about sensible defaults. It wants to make your life easier by reducing the number of decisions you have to make. JavaScript and its ecosystem often require more configuration, which can be a blessing or a curse, depending on your project's needs.

This, with MVC, is what powers developers to focus on code that brings value to the business , rather than spending days setting up solutions. Literally, you can build your MVP the same day you run `rails new` command!

## Real-World Success Stories

Now, let's dive into some real-world success stories to prove that Rails is alive and thriving.

**1. GitHub**

You've probably heard of GitHub, right? This platform for developers uses Ruby on Rails for its core functionality. It handles millions of users and repositories with ease.

**2. Basecamp**

Basecamp, the project management and team collaboration tool, is built on Rails. It's a shining example of how Rails can handle complex applications while maintaining simplicity.

**3. Shopify**

Shopify, the e-commerce giant, relies on Rails to power its online stores. Millions of businesses trust Rails to handle their sales and inventory.

## Challenges and Limitations

Of course, like any technology, Rails has its challenges and limitations. It may not be the best choice for every project. If you're building a super-high-performance app, Rails might not be the right tool.

If you are crunching peta-bytes of data in every breakfast, you might go with another language, or use Python with it's solid Data Ecosystem.

But from my experience, 99,99% of projects out-there don't need that! Most web applications are fine with mili-seconds latency rather than nano-seconds!

## Conclusion 🎉

So, is Ruby on Rails dead? Far from it! Rails is like a classic car that's still turning heads on the road. It may not be the flashiest, but it gets the job done reliably and efficiently.

Remember, the tech world is a diverse and ever-changing landscape. The right tool for the job depends on your project's needs. Don't count Rails out just yet; it's still an invaluable part of the developer's toolkit.

Until next time, happy coding and Rails on!

---

Cheers,

Hassen

![I want this car](/2023/10/collection-car.jpg)
