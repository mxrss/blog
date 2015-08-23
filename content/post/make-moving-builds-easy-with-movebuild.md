+++
date = "2014-02-16T20:00:56-07:00"
draft = false
title = "Make Moving Your Builds Easy With MoveBuild"
tags = ["Powershell", "Build Auotmation", "ALM"]
url = "/post/2014/02/16/make-moving-builds-easy-with-movebuild"
+++

## Make Moving Your Builds Easy With MoveBuild

Using a simple repeatable process you will be able to move your builds to each environment.

### Understanding the Move Workflow

As you move each build to each environment you want to create a pipeline. Where each build is considered a possible [[abbr title="Release Candidate"]]RC[[/abbr]]. Promotion of each build requires that it passes a series of tests, with each promotion the level of testing becomes more intense. For example, moving the build to [[abbr title="Development"]]Dev[[/abbr]] would only really require that the build can compile and pass a series of unit tests (if any are defined). Whilst, this is a simplistic test it gets you the necessary amount of feedback. As you progress to each level from [[abbr title="Development"]]Dev[[/abbr]] To [[abbr title="Quality Assurance"]]QA[[/abbr]] the tests become more stringent and the failure criteria becomes more easily defined.

![Sample Promotion](http://s3.amazonaws.com/mxrss-blog-images/63528178062.png)

The reason for this as we get closer to releasing a build to production we want to make certain at all costs that we will not have to rollback or at the very least make a rollback an infrequent event.

Having been bitten by bad deployments, I can tell you that its important to make your deployment the SAME *regardless* of where you deploy the binaries. This means that every time you move a piece a code it should be the same process, having differing ways of moving code from environment to environment could hide potential deployment problems.

> every time you move a piece a code it should be the same process, having differing ways of moving code from environment to environment could hide potential deployment problems

In addition your should make sure that the following are also adhered to.

* Build your binaries once
* Configuration changes should be minimal.
* Follow your deployment process in each environment, the same. Treat it like production.
* Accept a deployment failure and kill that [[abbr title="Release Canidate"]]RC[[/abbr]]

### A Simple Build Promotion Script

I needed to ensure that the process I am using is the same as I deploy to each environment, Below is a script that I came up with that ensures that each move is atomic and follows the same structure.  

<script src="https://gist.github.com/mxrss/8816312.js"></script>

#### Additional Resources
- [Continuous Delivery](http://en.wikipedia.org/wiki/Continuous_delivery)

- [Continuous Delivery: Reliable Software Releases through Build, Test, and Deployment Automation (Addison-Wesley Signature Series (Fowler))](http://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912)
