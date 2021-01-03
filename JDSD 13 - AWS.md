---
title: JDSD 13 - AWS
created: '2020-05-04T16:39:58.665Z'
modified: '2020-05-04T17:33:26.470Z'
---

# JDSD 13 - AWS

## Section Overview

* On-premise: When you had your own servers and machines.
* However, cloud hosting is more of a thing.
* In production, we want to make sure the elements can scale and be located in different parts in the world.
  * With AWS they take care of a lot of this for us.
  * Also Microsoft Azure and Google Cloud Platform

## Amazon Web Services

* S3
  * Object-storage service.
  * Each object is stored as a file with its metadata and id.
  * A key/id and with that you can access whatever object that id stores.
  * Any file or object that we may want (Images, videos)
  * Size limit: 5gb.
* DynamoDB
  * Databases.
  * Fast, no-sql database.
  * Manage scaling for you.
  * Key-value storage model.
* EC2
  * A bare-metal machine to run whatever we want.
  * Kind of like a digitalocean linux server.
* Lambda
  * You can run code for virtually any type of application or backend service.
  * You just provide it a function.
  * Lambda can scale and create multiple instances of a function so it can have many users.
* CloudFront
  * Speeds up distribution of our static files.
  * CDN - HTML/CSS/JS Files served quickly.
    * Cloudflare is a bit like this.
  * Makes our apps really fast because amazon has servers around the world.
  * Automatic HTTPS.

## Monolithic vs Micro Services

* Before we had all the pieces working together in one place, however it is now a lot easier to manage these machines and connect them.
* We can now have small machines that run stuff really quickly and really well.
  * They can be tested on their own, with different development teams and released at different times.
  * As long as they have a Service Level Agreement (SLA), that says 'have this data come to me but do what you want', then it all still works.

## Amazon Lambda

* Up to now, we've been responsible for every little piece. For provisioning and managing the resources.
  * However:
    * We're charged for keeping the server up, even when there are no requests.
    * We're responsible for maintenance and running of the server.
    * We have to worry about security.
  * At larger companies, they will have their own infrastructure team.
* Serverless allows us to build applications where we hand the cloud provider the code and it runs.
  * You also only get charged for what you use.
  * They can monitor what code gets executed and what gets used.
    * For example, the Christmas shopping period.
* To work Lambda:
  * A user makes a request.
  * We do a fetch call from our app, to a URL amazon lambda provides for us.
  * When we do this, it will run the function.
* The cloud provider would run a container with the function inside.
  * The one downside is that running the function takes a bit of time.
  * However, this function is only going to be run when we trigger it, other than that, Amazon don't charge for it.


### Serverless

* `npm install -g serverless`
* `sls create -template aws-nodejs`
  * This creates a boilerplate for using AWS Lambda with NodeJS.
* Serverless also works with other providers.

### IAM

* IAM controls permissions
* Once created a user role we then need: `sls config credentials --provider aws --key COPYPASTEACCESSID --secret COPYPASTESECRET`

### Deploying a Function

* In the .yml file, you can change:
  * `stage: dev`
  * `service: rankly-lamda
* With Lambda, the main principle is that everything should be run inside one function, 
* it should have one trigger as well.
* It is stateless. It does not remember anything for us.
* To deploy: `sls deploy`
  * To invoke: `sls invoke --function functionName` (This could cost you money)
  * To simulate locally: `sls invoke local --function rank`
    * Local only works with simple functions, might not work with S3 resources etc.
* Under the hood, if you change the events to http, it will use another AWS service called Gateway to give us an endpoint we can use to trigger the function

#### Part 2




