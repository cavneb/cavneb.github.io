---
layout: post
title: "Ember TDD with Canned Brunch"
date: 2014-01-30 18:47:43 -0700
comments: true
published: false
categories: 
- EmberJS
- JavaScript
- Testing
---

<div style="width: 288px;
      height: 400px;
      margin: 10px 30px 10px 0;
      float: left;
      background: transparent url(/images/posts/ember-brunch.jpg) 0 0 no-repeat;">
</div>

One of my new years resolutions is to become a responsible developer and start testing everything. I think I have been decent at it, but my Ember apps have been seriously lacking. I now feel that the "Ember Testing Story" has been nearly solidified and there are no more excuses.

I recently gave a [presentation on TDD with Ember](http://instructure.github.io/blog/2014/01/24/ember-run-loop-and-tdd/) at our [local Ember meetup](http://utahember.github.io/). This was a great experience and I learned a lot in the process. Now I would like to take it a step further and share how you test-drive an Ember application.

I wrote a couple of blog posts in the past which covered how to write a screencast viewer using Angular and Rails. In this post, I would like to walk through how to create the same application in Ember, but *without a backend*.

In this tutorial, I will be using an amazing build tool called Brunch with the well maintained [tapas-with-ember](https://github.com/mutewinter/tapas-with-ember) skeleton. I will also be using the [canned](https://github.com/sideshowcoder/canned) library to fake an API server.

<!-- more -->

## Install Brunch and create your app

First, you need to make sure you have **node**, **npm** and [**coffee-script**](https://npmjs.org/package/coffee-script) installed. Next, install brunch:

    $ npm install -g brunch

Create a new brunch project using the tapas-with-ember skeleton:

    $ brunch new gh:mutewinter/tapas-with-ember ember-screencasts

 Run the server

    $ cd ember-screencasts
    $ cake server

Now open up your browser to [http://localhost:3333](http://localhost:3333). You should see this:

![](/images/posts/ember-canned-brunch-1.png)

## Add in the canned API server

Our application will need to access an API which returns a list of screencasts. However, in this tutorial we are going to mock out the API using a node library called [canned](https://github.com/sideshowcoder/canned).

Install the npm and save it to your `package.json` file:

    $ npm install canned --save

Next, open up the Cakefile and make the following changes:

On line 9, require the 'canned' module.

On lines 32-37, add a function which runs the canned server. The server is configured to run on port 3000 and points to the *cans* folder for the API mock data.

On line 49 and 60, trigger the server to be run by calling the `cannedServer` function.

{% include_code Cakefile %}

Next, create the canned API responses. Add the following into `cans/api/_screencasts.get.json`

```json cans/api/_screencasts.get.json
[
  {
    "id": 1,
    "title": "Upgrading to Rails 4",
    "summary": "With the release of Rails 4.0.0.rc1 it's time to try it out and report any bugs. Here I walk you through the steps to upgrade a Rails 3.2 application to Rails 4.",
    "duration": "12:44",
    "link": "http://railscasts.com/episodes/415-upgrading-to-rails-4",
    "published_at": "2013-05-06T07:00:00.000Z",
    "source": "railscasts",
    "video_url": "http://media.railscasts.com/assets/episodes/videos/415-upgrading-to-rails-4.mp4",
    "created_at": "2013-05-23T02:32:11.498Z",
    "updated_at": "2013-05-23T02:32:11.498Z"
  },
  {
    "id": 2,
    "title": "Fast Rails Commands",
    "summary": "Rails commands, such as generators, migrations, and tests, have a tendency to be slow because they need to load the Rails app each time. Here I show three tools to make this faster: Zeus, Spring, and Commands.",
    "duration": "8:06",
    "link": "http://railscasts.com/episodes/412-fast-rails-commands",
    "published_at": "2013-04-04T07:00:00.000Z",
    "source": "railscasts",
    "video_url": "http://media.railscasts.com/assets/episodes/videos/412-fast-rails-commands.mp4",
    "created_at": "2013-05-23T02:32:11.530Z",
    "updated_at": "2013-05-23T02:32:11.530Z"
  },
  {
    "id": 3,
    "title": "Active Model Serializers",
    "summary": "The ActiveModel::Serializers gem can help you build JSON APIs through serializer objects. This provides a dedicated place to fully customize the JSON output.",
    "duration": "10:58",
    "link": "http://railscasts.com/episodes/409-active-model-serializers",
    "published_at": "2013-03-09T08:00:00.000Z",
    "source": "railscasts",
    "video_url": "http://media.railscasts.com/assets/episodes/videos/409-active-model-serializers.mp4",
    "created_at": "2013-05-23T02:32:11.544Z",
    "updated_at": "2013-05-23T02:32:11.544Z"
  },
  {
    "id": 4,
    "title": "Public Activity",
    "summary": "Learn how to easily add a user activity feed using the public_activity gem. Here I show both the default setup using model callbacks and a manual way to trigger activities.",
    "duration": "10:59",
    "link": "http://railscasts.com/episodes/406-public-activity",
    "published_at": "2013-02-13T08:00:00.000Z",
    "source": "railscasts",
    "video_url": "http://media.railscasts.com/assets/episodes/videos/406-public-activity.mp4",
    "created_at": "2013-05-23T02:32:11.559Z",
    "updated_at": "2013-05-23T02:32:11.559Z"
  },
  {
    "id": 5,
    "title": "Better Errors & RailsPanel",
    "summary": "Here we take a look at two tools to aid us in development: Better Errors which makes it easier than ever to debug exceptions, and RailsPanel, a Chrome extension to see Rails requests.",
    "duration": "8:06",
    "link": "http://railscasts.com/episodes/402-better-errors-railspanel",
    "published_at": "2013-01-25T08:00:00.000Z",
    "source": "railscasts",
    "video_url": "http://media.railscasts.com/assets/episodes/videos/402-better-errors-railspanel.mp4",
    "created_at": "2013-05-23T02:32:11.575Z",
    "updated_at": "2013-05-23T02:32:11.575Z"
  }
]
```

Finally, start up the server.

    $ cake server
      Canned listening at port 3000
      Running Brunch in development environment
      30 Jan 21:09:25 - info: application started on http://localhost:3333/
      30 Jan 21:09:27 - info: compiled 14 files and 1 cached into 3 files, copied 12 in 1505ms 

Once it's up, you should be able to access the API at [http://localhost:3000/api/screencasts](http://localhost:3000/api/screencasts).

## It's EMBER TIME!

Let's approach development using TDD, or Test-Driven Development. I admit that I am still learning how to do this effectively in front-end development. Basically what I would like to do is write a failing integration test with the expected behavior and then fix the test by writing code. Afterwards, I can refactor to make it right.

### Test: Header exists

Let's start off by creating an integration test called `screencasts_test.coffee` in the folder `test/integration`.
