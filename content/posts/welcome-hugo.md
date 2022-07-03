---
title: "Welcome Hugo"
date: 2019-08-26T20:09:05-07:00
tags:
  - announcement
---

I’ve moved my blog from one static site generator to another. I used to use a [stripped clone](https://github.com/kyzn/mini-plerd) of [Plerd](https://github.com/jmacdotorg/plerd) to generate html files. To be clear, that was a nice setup! Now I’m using [Hugo](https://gohugo.io) to generate static content, from the very same markdown files.

I never was able to maintain mini-plerd (the stripped Plerd clone I mentioned above) as I hoped. Plerd upstream got updates, and I missed it. I didn’t even have markdown tables until very recently! I should have used the original Plerd.

Hugo comes with [many theme choices](https://themes.gohugo.io/). You need to pick one and go. Themes are easy enough to configure, and looks like easy enough to switch as well.

Another change I made was the way I deploy the blog. I used to upload my files to a DigitalOcean droplet, and serve my blog via nginx. That alone cost me $5/month. I could have used a storage like AWS S3 or DigitalOcean Spaces too. I’m now using [GitHub pages](https://pages.github.com), where you put your files into a repository and GitHub serves it for free. It works with custom domains too, so that’s great.

So far I love Hugo. It's very easy to write new posts in local development environment.
