# Z-Insights Engineer Homework

## Overview

Write a program that reads and processes a '\n' delimited input file, where each line is a JSON encoded message in one of the following formats.

### Event

    {
      "timestamp": 1437055155340,
      "stream": "frontleaf",
      "userId": "U1",
      "accountId": "A1",
      "event": "login"
    }

This message represents an event that occurred in a customer's system.

### Identification

    {
      "timestamp": 1437055157310,
      "stream": "frontleaf",
      "userId": "U1",
      "userName": "greg.mcguire@zuora.com",
      "userData": {
        "role": "admin"
      },
      "accountId": "A1",
      "accountName": "Zuora",
      "accountData": {
        "status": "paying",
        "plan": "enterprise"
      }
    }

This message identifies an account and user from the customer's system.

## Processing

While reading through the file, the program should create and maintain state, either in memory or in an external datastore of your choosing. State should include all User and Account data, as well as counts (by day) of each type of Event that has occurred for that User and Account. 

Each message identifies our customer (indicated by "stream".) Each stream's data is isolated and unique from every other stream. You can assume each stream begins life empty, with no Accounts, Users, or Events, and is auto-created as you encounter data. Each message also carries a timestamp (indicated by "timestamp") which is milliseconds since the epoch. You can ignore timezones (assume UTC.)

Each User belongs to one Account. A User cannot belong to more than one Account; you should discard messages that attempt this. Account and User IDs are unique from their peers in a stream. User and Account names are not.

Data *is not* guaranteed to be in temporal order while processing. Event messages can occur prior to Idenfication messages for any given User/Account.

After processing, your program should output to stdout or file a summary of activity in JSON format, keys sorted alphanumerically. The summary must include the daily event frequency for each type of Event, across all Accounts segmented by their `status`, and all Users segmented by their `role`.  For example:

    [{
      "accounts": [
        "paying": [
          "2015-07-15": {
            "login": 143,
            "clicked_something": 1494
          }
        ]
      ],
      "stream": "frontleaf",
      "users": [
        "admin": [
          "2015-07-15": {
            "login": 73,
            "clicked_something": 291
          }
        ]
      ]
    }, ...]


## Validation

Sample input and output files are included.

We will test your program with at least 1 million events, and run it on a machine with approximately 2GB of RAM.

## Requirements

 1. Can be written in language of your choosing
 1. Include a README describing how to build and run your program
 1. Build and run on an Ubuntu 14.04 machine (`apt` or language tooling for dependencies)
 1. Present correct output

## Bonus points

 1. Survive failure: if your program and/or database is force killed, the eventual output should still be correct
 1. Avoid reprocessing from the start in the case of a restart after a failure
 1. Flag invalid messages (User on multiple accounts)

