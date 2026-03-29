# Environments

## Core Concept

The Project should be easy to setup enywhere in different environments by changing only .env / IaC files.

Every environment should be able to run in single line command, optionally specifying `.env` file path.

## Environments

- `local` - local development environment
- `test` - environment for running tests
- `dev` - development environment
- `production` - production environment

## Environment Variables

Use `.env` files in order:

- .env - global
- subproject/.env - subproject specific