---
title: 'Ultra-Disposable Testing Environments With Docker'
author: Mark Stenglein
date: '2017-12-18'
categories:
  - Development
tags:
  - Docker
  - Programming
---

Often times I find myself wanting to try out some piece of software from Github, a tutorial, or just have a fresh environment to work in that will avoid causing damage to my computer’s local environment. Typically I would use Vagrant to spin up a virtual machine, and delete it after. This is a time consuming and tedious process.

Docker provides a convienent method which gives you a clean environment that is destroyed when you finish with it.

First, [install Docker](https://docs.docker.com/engine/installation/). If you run Ubuntu 17.10, you will likely need to use either the Edge or Xenial PPA. As of when I’m writing this, Docker has not been officially released for 17.10.

Next, download the image you want to run:

```bash
docker pull ubuntu:latest
```

For conviencence, you can optionally add the following to your `.bashrc` file:

```bash
alias ubuntu='docker run -it ubuntu:latest /bin/bash'
```

After logging out and in to get the new bashrc (or just `source` the config), you should be able to run `ubuntu` and be greeted with a prompt.

{{< figure src="/img/docker-disposable-env/docker-env.png" >}}
