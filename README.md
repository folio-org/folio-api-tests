# folio-api-tests

## Running API tests

The tests can be run directly from the command line by using [Newman] (https://github.com/postmanlabs/newman)  To run Newman, make sure that you have NodeJS v4 or higher installed. 
The easiest way to install Newman is to use NPM

```terminal
$ npm install newman --global;
```
The `newman run` command is used to run a specific collection against a given environment.

```terminal
$ newman run <path_to_postman_collection_file> -e <path_to_environment_file>;
```