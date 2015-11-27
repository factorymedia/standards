# Coresites Deployment Guide

## Deploying

To deploy, either use [jenkins](http://jenkins.fm-ops.com)

Or the command line: `bundle exec cap <stage> deploy`

## For QA

For QA deploying, the first time you deploy a QA server will be created. It will post the domain name to slack, which will be `{branch_name.downcase.gsub('/', '__')}.qa.{site}` (Lower case branch name, slashes changed to double underscores)

The QA box will be automatically destroyed when the pull request is closed

## Databases

To copy databases from production to staging or QA:

* `bundle exec cap <qa/staging> deploy:copy_db SITE=<site>`

