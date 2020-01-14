# Prismatopia

NOTE: THIS IS A WORK IN PROGRESS, CHANGING RAPIDLY, NOT STABLE, USE AT YOUR OWN RISK UNTIL THIS SENTENCE IS REMOVED FROM THE README!

An API stack combining a bunch of trendy components (circa 2020): Apollo Server, Prisma, OAuth, OpenID Connect, JWT, Postgres, Docker, Typescript, AWS and more!

## How to use this repository

Prismatopia is a reference implementation, meaning you can clone it, configure, run it and play with it. But if you want to use it for your own purposes, you need to heavily modify it. You make a copy of this repository and that copy becomes the backend for your application. You'll end up customizing your copy of this repository with your resolvers, schemas and other stuff. This repository is simply an empty vessel for you to fork or otherwise copy as the starting point for your API stack.

## The Stack

This API is built as a very specific stack of technologies. This is about as opinionated a stack as it gets. There no options, other than configuring the existing stack components or swapping them out in your own copy. Enjoy!

Here are the technologies in this stack...

* AWS
  * Handles networking (ALB, VPC, etc.) and container management (ECS)
* Apollo Server 2
  * Provides a GraphQL server for resolvers, which is where business logic lives
* Prisma
  * Provides an ORM to translate from Graphql to Postgres, Apollo resolvers mainly call a Prisma Client to access data
* Postgres
  * Provides persistent storage for data, this is managed by AWS RDS in production but is run in a container during local development
* Github
  * Provides a place to hold the source code and Docker images

## Directories

The various components of this stack are organized into folders to help keep everything neat and tidy.

### Root

`.env`
You don't see this because you'll need to create it and load it up with your specific configurations. To get Prismatopia running for reference, you should create a `.env` file in the root directory that looks like this:

```
APPLICATION_NAME=donuts
ENVIRONMENT_NAME=stage

OAUTH_TOKEN_ENDPOINT=https://<your okta domain>.okta.com/oauth2/default
OAUTH_CLIENT_ID=<your okta client id>

PRISMA_MANAGEMENT_API_SECRET=<a big long gnarly secret string used to access the prisma management api>

PRISMA_ENDPOINT=http://localhost:7000/

PRISMA_SECRET=<another big long gnarly secret string used to access your application's prisma api>
```

`sourceme.sh`
A simple little script that will load the environment variables in `.env` into your shell environment. You need to source this for it to work: `source ./sourceme.sh`

`docker-compose.yml`
A Docker Compose file that brings Prismatopia up locally for you to play with and develop against. Use it by running `docker-compose up`

`.gitignore`
Very important to ensuing that you don't accidentally commit `.env` into your repository!

`Makefile`
Oh how do we love the Makefile! So handy! Anything you need to do with Prismatopia can be done from the make file...

### The Layers

* [The Apollo Layer](apollo/README.md)
* [The Prisma Layer](prisma/README.md)
* [The AWS Layer](cloudformation/README.md)

## Running locally

1. Create the `.env` file
2. Source the `.env` file: `source ./sourceme.sh`
3. Run Prismatopia: `docker-compose up`
4. Deploy the Prisma schema: `make prisma-deploy`
5. Generate a token: `make prisma-token`
6. Play: `http://localhost:7000`

## Running in AWS

1. Create the `.env` file
2. Source the `.env` file: `source ./sourceme.sh`
3. Edit the `cloudformation/params.json` file to your liking
4. Deploy to AWS: `make cf-aws-deploy-all`
5. Generate a token: `make prisma-token`
6. Play: `https://prisma.<your domain>`