---
layout: post
title: "docker gitlab-runner配置"
date: 2016-09-22
excerpt: ""
tags: [docker,gitlab]
comments: true
---

<pre><code>Run gitlab-runner in a container:

docker run -d --name gitlab-runner --restart always \
  -v /data/gitlab-runner/config:/etc/gitlab-runner \
  gitlab/gitlab-runner:latest

Register the runner:

docker exec -it gitlab-runner gitlab-runner register
Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/ci):
https://gitlab.example.com/ci
Please enter the gitlab-ci token for this runner:
1234566e1234f6d84e9e191234526e
Please enter the gitlab-ci description for this runner:
[9f86b7104dc7]: gitlab-ci-runner
Please enter the gitlab-ci tags for this runner (comma separated):
gitlab-ci-runner
INFO[0046] 5a32a66e Registering runner... succeeded     
Please enter the executor: parallels, docker, docker-ssh, ssh, shell:
docker
Please enter the Docker image (eg. ruby:2.1):
ruby:2.1
If you want to enable mysql please enter version (X.Y) or enter latest?
latest
If you want to enable postgres please enter version (X.Y) or enter latest?
latest
If you want to enable redis please enter version (X.Y) or enter latest?
latest
If you want to enable mongo please enter version (X.Y) or enter latest?
latest
INFO[0148] Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!</code></pre>