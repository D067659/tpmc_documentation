# User Service

The User Service's responsibility is to handle the authentication of the user at the frontend with the micro services Master Service and Service Provider. Processwise the Frontend is logging into the User Service and receives a token which it can use for the other services. The other services validate the token at the user service and receive the user's ID to retrieve information linked to the user.

## Endpoints
To provide this service two endpoints exist:

`/login/`

The login endpoint receives a POST request with the username and password. It returns a token. The Frontend will make the login request and hand the tokens to the other services.

`/api/auth/`

The authentication endpoint receives a get request without content but the token authentication in the header. If the token was valid it returns a successful reponse and the user ID of the token. This way the service can verify that the user is authenticated directly. The user ID can be used as a foreign key to retrieve user specific data.

## Implementation
The login service is provided by Django Restframework's (DRF) Basic Authentication and User model. The authentication via the authentication endpoint is done by the DRF's TokenAuthentication where the user ID is retrieved from the user attached to the DRF request. The users are saved with hashed passwords in a dedicated PostgreSQL database.
