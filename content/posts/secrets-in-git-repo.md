---
title: "Secrets in Git Repo"
date: 2023-11-07T23:40:51+09:00
tags:
  - security
  - git
---

It’s no secret that keeping secrets in a git repository is a bad idea. I want to do the mental exercise of imagining what can go wrong. I need to limit this to a point so as not to come up with far-fetched attack vectors like “Aliens invade the earth and they figured out all our prime numbers”. So here is a quick-list of some things that can go wrong when you add secrets to your repo.

1. Some devs may assume the secret is not a real secret because it’s in a repo. This may result in **sharing the secret in other channels like email or Slack** by mistake. Likewise, **the secret may be logged** by mistake and may show up in even more places.

2. When an employee leaves your company, **they become an outsider who knows your secrets**. You may choose to rotate your keys every time an employee leaves, but there may be an easier alternative. If you're using a vault, you can set up which employees have access to which secrets. You also get access logs so that's another plus.

3. If the repo is leaked (either by mistake or on purpose), **your secret is leaked** too. The secret you put into the repository lives in all computers that clone the repository. This increases the chance of the leak happening.

4. **You may lose reputation** when your code with secrets is leaked and it makes into news. **You may be sued by some customers** who saw this and thought you don't have good enough security or you broke a policy.

5. If your code is fed into a LLM (Large Language Models), **attackers may retrieve it with prompt injection**. They can tell ChatGPT how their grandma used to tell them passwords when they were trying to fall asleep.

