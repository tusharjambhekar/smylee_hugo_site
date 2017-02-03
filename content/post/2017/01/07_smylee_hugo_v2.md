+++
title = "Updated Hugo Site Automation Generator"
date = "2017-01-07T17:00:00-08:00"
toc = false
draft = true
categories = ["Step-by-Step Guides", "AWS"]
tags = ["AWS", "Lambda", "IAM", "SNS", "S3", 'GitHub"]
+++

I've decided to finally go back and revamp my original hugo setup which can be found in the [Hugo Site Auto Generated by Lambda](/post/2016/05/01_hugo_lambda/) post. The issue with that previous setup was if I had multiple posts (markdown files), the S3 event would trigger the lambda function as many times as the number of files uploaded, when I only needed it to run once after all were uploaded.

My new plan was to publish the markdowns to github and have a lambda function trigger to pull the files and put into S3. The problem with this is I'm using python and the way I need to package the lambda function will not allow both a git binary and the python git library in the same root because they are named the same. While it may be possible with another language supported by lambda, at the moment,  I'm not too interested in investing time into using another language.

So I went another route and decided to use travis. After a quick setup and a small little python unittest, the code from my git push is being uploaded to S3 - as so long as my tests pass. 

With this now in place, I decided to write a python test to check the format of the hugo headings in the md files.

-----

2017-01-14:
Earlier this week I realized that I could just download the zip file from GitHub in the lambda function, there is no reason to clone it.

Additionally with downloading it directly, I should put it with the hugo function.

I found GitHub has an SNS integration. So will be using that for triggering the 