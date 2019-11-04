# Credit Card Registry application

Create a simple Credit Card Registry backend application, designed for storing
the credit card information of the clients. The application does not have to
provide a solution for payments, it is just a registry.

## Functional expectations

### Credit Card Registry

The application should store the following data in a database:

- Type of the card, you can find the complete list [here](https://www.erstebank.hu/hu/ebh-nyito/mindennapi-penzugyek/beteti-kartyak)

- Card number
- Expiration date, MM/YY format
- Hash based on the card number, the expiration date and the CVV code
- Flag to check if the card has been blocked or not
- Name of the owner
- Contact details (list):
  - Type of contact (Email/SMS)
  - Contact

Storing the CVV code is not necessary

### Adding a New Card

The application should have a feature designed to add a new card to the registry.
It should be a RESTFUL service, the input parameters should be sent in JSON format.
In case of success it should respond with HTTP 200 code, if an error occurs,
it should return HTTP 500;

Service URL: POST /ecards

Description of the parameters:

| Level | Parameter   | Data type | Description                    |
| ----- | ----------- | --------- | ------------------------------ |
| 1     | cardType    | String    | Card type                      |
| 1     | cardNumber  | String    | Card number                    |
| 1     | validThru   | String    | Expiration date                |
| 1     | CVV         | String    | CVV code from the back of card |
| 1     | owner       | String    | Owner's name                   |
| 1     | contactinfo | Array     | Contact details of the Owner   |
| 2     | type        | String    | Type of contact                |
| 2     | contact     | String    | Phone number or email          |

### Querying a card

The application should have a feature designed to query an already existing card by Card Number. It should be a RESTFUL service, with one input that is a path parameter. In case of success it should respond with the credit card's details, if an error occurs it should return HTTP 404;

Service URL: GET /ecards/{cardNumber}

Description of responses:

| Level | Parameter   | Data type | Description                  |
| ----- | ----------- | --------- | ---------------------------- |
| 1     | cardType    | String    | Card type                    |
| 1     | cardNumber  | String    | Card number                  |
| 1     | validThru   | String    | Expiration Date              |
| 1     | disabled    | Boolean   | Is it blocked/disabled?      |
| 1     | owner       | String    | Owner's name                 |
| 1     | contactinfo | Array     | Contact details of the Owner |
| 2     | type        | String    | Type of contact              |
| 2     | contact     | String    | Phone number or email        |

### Validating a card

The application should have a feature designed to check the validity of the given card's data received as a parameter. It should be a RESTFUL service, both the request and the response should be in JSON format. It should check:

- If the data from the request is the same as the data in the database (cardType, cardNumber, validThru)
- The card is not blocked/disabled
- The hash stored in the database is the same as the hash that is created with the data received in the request (cardNumber,validThru,CVV)

All three checks should have 'VALID' in the response if the checks are successful, if any result in error it should be 'INVALID'. If the data provided is bad, it should return HTTP 500 status code.

Service URL: POST /ecards/validate

Description of the parameters:

| Level | Parameter  | Data type | Description                      |
| ----- | ---------- | --------- | -------------------------------- |
| 1     | cardType   | String    | Card type                        |
| 1     | cardNumber | String    | Card Number                      |
| 1     | validThru  | String    | Expiration Date                  |
| 1     | CVV        | String    | Card Validation code of the card |

Response structure

| Level | Parameter | Data type | Description                               |
| ----- | --------- | --------- | ----------------------------------------- |
| 1     | result    | String    | Result of validation check: INVALID/VALID |

### Blocking a card

The application should have a feature designed to block a card. It should be an irreversible action, going in one-way.  It should be a RESTFUL service that can change the blocking flag in the database of the card that has been sent as a parameter. In case of success the response should be HTTP 200 status code, if an error occurs it should send HTTP 404.

Service URL: PUT /ecards/{cardNumber}

## Non-functional requirements

- The application should be written in Java EE or Spring framework
- The database should be a relational database  (the suggested one is ORACLE RDBMS)
- The application should utilize logging, the suggested format is the slf4j based logging system such as logback
- The application should use gradle for building the app
- Online swagger documentation should be created for the software
- It should use git as version controlling system

## To be shipped

- Source code of the application and the gradle build script used to create the runnable binary
- SQL script that builds the database for the application
- Instruction on how to setup/run the application

### For plus points

- The application should have a *healthcheck* service that returns a {"status":"ok"} value, with HTTP 200 status code
- The application should run in a docker container
- The application should be run using docker-compose
- The application and the container should be built automatically using a Jenkins multibranch pipeline
