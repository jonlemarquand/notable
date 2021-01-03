---
title: JDSD 15 - Continuous Integration/Continuous Delivery
created: '2020-05-07T07:17:10.494Z'
modified: '2020-05-07T09:39:49.369Z'
---

# JDSD 15 - Continuous Integration/Continuous Delivery

## Continuous Integration, Delivery, Deployment

* Continuous Integration is a way for developers to use a shared repository throughout the day
  * Code PR -> Tests -> Build -> Merge
  * You can find errors quickly because each change is small.
  * This process makes sure the code of the individual developers meet a certain standard.
  * This is rather than the day before release, everyone merges their code and it has to be debugged.
  * A great emphasis on testing automation to check when a new commit is integrated into the main branch.
* Continuous Delivery is the practice of keeping your codebase deployable at any point.
  * Always keep things ready.
  * You might have someone who is a QA or a product owner who checks the app has the features needed.
  * Makes sure you can release changes to the customer quickly.
  * Small but often updates.
* Continuous Deployment is also about keeping your app deployable at any time.
  * Deployment goes straight into production.
  * If acceptance tests are automatically passed, it is automatically deployed.
  * Think of this like github pages.
    * If we merge into master, the github page will be updated.
  * Every change that passes goes into production.
    * Only a failed test will stop the app going into production.

## Building Great Software

* Continuous Integration should happen right at the start of the project.
  * Old software practices use to:
    * Everyone works on a feature -> Push to a branch -> Friday merge to master branch
      * This was massively stressful 
  * Pull requests + merge to master daily.
    * If you leave the code sitting on a branch too long, you expose yourself to more risks.
    * By integrating your code early, you reduce the scope of the changes.
    * Typescript/Lint/Tests -> Prettier -> Github -> circleci -> Code Review -> Merge

## CircleCI

* They own the servers and run the tests for us.
* New folder: `.circleci` > `config.yml`

```yml

version: 2
jobs:
  bobby:
    docker:
      - image: circleci/ruby:2.4.1
    steps:
      - checkout
      - run: echo "A first hello"
workflows:
  version: 2
  bobby_sally:
    jobs:
      - bobby
      - sally

```
* Defining the version.
* THen we define jobs, what we want the server to do.
* We want it to do a build, with a docker image

### Continuous Integration

```yml

version: 2
jobs:
  build:
    docker:
      - image: circleci/node:8.9
    steps:
      - checkout
      - run: echo "npm installing"
      - run: npm install
      - run: CI=true npm run build
  build:
    docker:
      - image: circleci/node:8.9
    steps:
      - checkout
      - run: echo "testing stuff"
      - run: npm install
      - run: npm test
  hithere:
    docker:
      - image: circleci/node:8.9
    steps:
      - checkout
      - run: echo "helloooooooo!"
workflows:
  version: 2
  build_test_and_lint:
    jobs:
      - build
      - hithere
      - test:
        requires:
          - hithere
```
* In this examples, build and hithere will run in parallel on different containers, but the test container will not run until hithere passes.
* No code review is really necessary until all the circle tests past.
* What about running tasks before we even commit?
  * Enter Prettier
    * Prettier only runs on the files that we've changed.
    * It's a nice tool to make sure eveyrthing looks consistent
* Write Code > Lint/Typescript/Tests > Pre-Commit Hook (Prettier) > Pull Request on Github > CircleCI runs the test > It passes and goes to a code review > If they accept they merge changes > Deploy to staging > Acceptance Test > Deploy to production > Smoke Test
  * Staging mimics production as much as possible
  * Acceptance Test - Product Manager/Senior Person checks the app
  * Smoke Test - A good way to be alerted if something goes wrong.
