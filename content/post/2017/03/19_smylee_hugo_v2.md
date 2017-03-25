+++
title = "Updated Hugo Site Automation Generator"
date = "2017-03-19T17:00:00-08:00"
toc = false
draft = false
categories = ["AWS", "Site Update"]
tags = ["AWS", "Lambda", "IAM", "SNS", "S3", "GitHub", "API Gateway", "Python"]
+++

I've decided to finally go back and revamp my original hugo setup which can be found in the [Hugo Site Auto Generated by Lambda](/post/2016/05/01_hugo_lambda/) post. The issue with that previous setup was if I had multiple posts (markdown files), the S3 event would trigger the lambda function as many times as the number of files uploaded, when I only needed it to run once after all were uploaded.

My new plan was to publish the markdowns to github and have a lambda function trigger to pull the files and put into S3.

I wanted to use the git clone, but I cannot install `git` on the container lambda is running on.

I did try to package git binary with the lambda package, but the folder name `git` was conflicting with the python package `git` because of the way python files must be packaged for lambda. Another language might support it but I'm learning to write lambda functions in other languages from other means.

So I setup travis. After a quick setup and a small little python unittest, the code from my repository is being deployed to S3 - as so long as my tests pass. giThe test I wrote, checks the format of the hugo headings in the md files.

Shortly after, I realized that I do not need to clone, I just need to download, and I can use the zip download link from github.

I removed the deploy from travis and added the github signature check lambda function to an API Gateway endpoint and hooked that up to a github Webhook so push events to the repo will trigger the github signature check lambda function.

Upon success of the github signature check, it will send a `done` message to sns which will trigger the lambda function that does the github download and deployment of my hugo site.

All that description is simply demonstrated with a diagram:

<img src="http://cdn.smylee.com/images/2017/03/smylee_com_github_workflows.png" alt="Automating Smylee.com updates with Hugo workflow diagram" title="Automating Smylee.com updates with Hugo workflow diagram">