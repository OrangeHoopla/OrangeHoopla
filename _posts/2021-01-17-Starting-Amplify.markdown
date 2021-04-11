---
title:  "Starting Amplify"
date:   2021-01-17
categories: [EZPZMC]
tags: [aws,amplify,react]
---
Wow never thought I would be doing it this way but it seems AWS Amplify is the best option to do our backend for this project or maybe as middleware since amplify is only a year old it seems to have a few bugs when using it for the backend. The best way to describe it is as an addon for React that makes it(and I imagine other future supported frameworks) act like terraform for AWS.
getting started

`npm install -g @aws-amplify/cli`

after that we can run `amplify configure` where it opens up our browser and prompts use to make a IAM user that our project will use.

next starting the React project we can run `npx create-react-app EZPZMC-Amplify-React`
`cd EZPZMC-Amplify-React`
then to make our project a aws amplify project we run `amplify init` and thats it we've made an AWS Amplify React project

<h2>EZPZMC Goal</h2>
The goal of this project is to have a service hosting platform that when users login and pay they can access a ready to go minecraft server. Eventually the goal is to incorporate Kubernetes and Docker so that users can add customized mod/server packs.

<h2>EZPZMC scope</h2>
The scope of this project is to have AWS act as every part. Using Cognito for the user pool and the Authentification along side federated access from Google and possibly other social media verification. Lambda Function's to call either EKS or directly create EC2 isntances with alarms. with Lambda functions also calling all instances that are associated with the user