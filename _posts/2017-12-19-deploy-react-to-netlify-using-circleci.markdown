---
layout: post
title:  "Deploy React Single-Page App to Netlify using CircleCI"
date:   2017-12-15 06:00:00
author:
  display_name: Andrew Lombardi
  login: kinabalu
  email: andrew@mysticcoders.com
  url: http://www.mysticcoders.com
  twitter: kinabalu
comments: true
---
At Mystic we are big fans of the ReactJS framework. We've given a few talks about it at 
tech conferences over the last year as well as you can see from the [event page](/events.html). In an effort to 
using continuous integration with most things we do, [CircleCi](https://circleci.com) has made it's way
into our stack for it's ease of use. 

The pastebin we wrote years ago [Mystic Paste](http://mysticpaste.com) was converted to use ReactJS and Golang 
about a year ago, and is sitting on a hosted VPS. As an exercise in flexibility we wanted to offload the 
app to a "serverless" backend. The first step in doing so was migrating the frontend to be served from 
a static page host like [Netlify](https://netlify.com). A bit of searching came up with [instructions on
how to achieve this](https://www.netlify.com/blog/2016/07/22/deploy-react-apps-in-less-than-30-seconds/) 
"in 30 seconds no less". While this worked fine, it does require a deploy step that would rather be avoided 
manually.

Enter CircleCI. We are using 1.0 because it's just simpler, and may upgrade this doc with instructions for 2.0.
<!--more-->

Since this is React, I prefer to use Yarn over npm for our projects. npm is the default in Circle so we'll have 
to make a few changes. 

First thing is overriding the dependencies and test phases to run `yarn` instead of `npm`.

``` yaml
dependencies:
  override:
    - yarn
test:
  override:
    - yarn test
```

Due to the override, we'll also have to add the `.bin` directory in `node_modules` like so.

``` yaml
machine:
  environment:
    PATH: "${PATH}:${HOME}/${CIRCLE_PROJECT_REPONAME}/node_modules/.bin"
```

And of course to make the process run quicker, the cache should be updated to follow `yarn` conventions:

``` yaml
dependencies:
  cache_directories:
    - ~/.cache/yarn
```

Our deploy steps are where the magic with Netlify happens. If you are familiar at all with CircleCi then adding 
a deploy step should be simple. We'll need to either specify a branch or a tag (depending on how your deployment works).
Below is an example from the [mysticpaste-react](https://github.com/kinabalu/mysticpaste-react) project.

``` yaml
deployment:
  production:
    branch: production
    commands:
```

Netlify deployment stores two files that are important.

![CircleCi Environment Variables](/images/article_assets/circleci-environment-vars.png)

``` bash
- REACT_APP_ENV=production yarn run build
- mkdir ~/.netlify &&  echo -e "{\"access_token\":\"${NETLIFY_ACCESS_TOKEN}\"}" > ~/.netlify/config
- echo '/*    /index.html   200' > build/_redirects
- echo -e "{\"site_id\":\"${NETLIFY_SITE_ID}\",\"path\":\"`pwd`/build\"}" > build/.netlify
- cd build && netlify deploy
```