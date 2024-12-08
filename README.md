# JWKS Server with AES Encryption and User Registration

This project extends a basic JSON Web Key Set (JWKS) server by integrating AES encryption for private keys, user registration capabilities, and logging of authentication requests. It uses SQLite for secure storage of private keys and user data, ensuring persistence across server restarts and protecting against SQL injection attacks through parameterized queries.

## Project Overview

This project enhances the security and functionality of a JWKS server by:
- Encrypting private keys in the SQLite database using AES.
- Adding a user registration endpoint that generates secure passwords for new users.
- Logging authentication requests to track user access.
- Optionally implementing a rate limiter to prevent abuse of the authentication endpoint.

## Objective
- Implement AES encryption for private keys.
- Store user credentials and hashed passwords securely in SQLite.
- Log authentication requests for monitoring purposes.
- Implement an optional rate limiter to control requests to the `/auth` endpoint.

### Key Features
- **AES Encryption for Private Keys**: Private keys are encrypted using AES encryption and stored securely in SQLite.
- **User Registration**: Users can register by providing a username and email, receiving a securely generated password in return.
- **Authentication Logging**: Logs each authentication request with the user's IP address, timestamp, and user ID.
- **Rate Limiting (Optional)**: Limits requests to 10 requests per second on the `/auth` endpoint to protect against DoS attacks.
- **SQLite Integration**: Stores both private keys and user credentials in SQLite for persistence and security.
- **Robust Testing**: Validated with a test suite achieving high code coverage.

## Project Structure

- **main.py**: Core server code with key functionality:
  - `POST /register`: Registers a new user, generates and returns a secure password, and stores the user data in the database.
  - `POST /auth`: Authenticates a user and logs the authentication request. Optionally, the rate limiter applies to prevent abuse.
  - `GET /.well-known/jwks.json`: Returns the public keys in JWKS format.
- **totally_not_my_privateKeys.db**: SQLite database for logging authentication requests (IP address, user ID, timestamp).
- **requirements.txt**: Lists dependencies, including `cryptography` for AES encryption and `pyjwt` for JWT management.
- **test_server.py**: Contains unit tests for validating the endpoints and ensuring security and functionality.


## Setup and Usage

### Prerequisites
- Python 3.x
- Required Python packages listed in `requirements.txt`.

### Installation

1. Clone the repository.
2. Install dependencies:
   ```bash
   pip install -r requirements.txt

# Running the Server
To start the server on localhost:8080, run:
python main.py


# API Endpoints
POST /register: Registers a new user, generating a secure UUIDv4 password and returning it to the user.
Request body:
{
    "username": "NewUser",
    "email": "user@example.com"
}

Response:
{
    "password": "generated_uuidv4_password"
}
POST /auth: Authenticates a user and returns a JWT. Requests over 10 requests per second are rate-limited (if implemented).

Request body:
{
    "username": "NewUser",
    "password": "generated_uuidv4_password"
}
GET /.well-known/jwks.json: Returns a JSON Web Key Set containing public keys for JWT verification.

# Logging and Rate Limiting
Authentication requests are logged in the auth_logs.db table, including the request IP address, user ID, and timestamp.
Rate limiting is applied to the /auth endpoint, restricting requests to 10 requests per second. Requests that exceed this limit will receive a 429 Too Many Requests response.


# Testing
Gradebot Blackbox Testing
To ensure functionality, run the Gradebot client in the same directory as main.py and totally_not_my_privateKeys.db:

gradebot.exe project3


# Unit Testing and Coverage
Run unit tests and check coverage:

coverage run -m unittest discover
coverage report


# Screenshots
[Gradebot Output](Gradebot.png): Shows Gradebot rubric table and points awarded.

[Test Coverage Report](TestSuite Coverage.png): Shows the test coverage percentage and coverage details for test_server.py.
