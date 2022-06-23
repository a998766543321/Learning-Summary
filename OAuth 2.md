# OAuth 2

* OAuth 2 is referred to as an **authorization framework**
* OAuth 2 implies multiple possible authentication flows
* OAuth 2 doesn’t assume a specific implementation for tokens

## OAuth 2 components
1. The resource server
2. The user (also known as the resource owner)
    1. A user generally has a username and a password that they use to identify themselves.
3. The client
    1. Client credentials is not equals to user credentials
4. The authorization server

## Authentication flows
* OAuth 2 offers multiple possibilities for obtaining a token, called grants

### OAuth 2 grants
1. [Authorization code](#authorizationCode)
    * We choose this grant type when the user doesn’t trust the client and doesn’t want to share their credentials with it.
    * Mind that an authorization code can only be used once for making a request for access token.
2. [Password](#password)
    * You should apply this only if you can trust the client
    * less secure compared to authorization code since the user needs to share user credentials with the client
3. [Refresh token](#refreshToken)
    * Apply to single sign-on applications
4. [Client credentials](#clientCredentials)
    * choose this grant type when the client needs to call an endpoint of the resource server that isn’t a resource of the user
    * used in system to system authorization
    * No user credentials and refresh token invlovled
    * Make sure that you don’t offer it access to the same scopes as flows that require user credentials.
    * Otherwise, you might allow the client access to the users' resources without needing the permission of the user

#### <a name="authorizationCode">Authorization code flow implementation</a>
1. The user makes the authentication request with the authorization code grant type
    * The user doesn’t send the credentials to the client but to the authorization server directly
    * The server returns an authorization code to the client
2. The client obtains an access token with the authorization code grant type
    * The client sends the authorization code obtained in step 1 to the authorization server
3. The client calls the protected resource with the authorization code grant type to the resource server
    * Attach the access token in the authentication header of the resource request  

#### <a name="password">Password flow implementation</a>
1. The user give the user credentials to the client
2. The client makes a request to the authorization server with the user credentials using the password grant type
    * The client obtains the access token
3. The client calls the protected resource with the password grant type to the resource server
    * Attach the access token in the authentication header of the resource request

#### <a name="clientCredentials">Client credentials flow implementation</a>
1. The client requests an access token with the client credential grant type
    * send the request with the client credential only
    * No user credential
2. The client uses an access token to call resources with the client credential grant type to the resource server
    * Attach the access token in the authentication header of the resource request

#### <a name="refreshToken">Refresh token flow implementation</a>
1. The client received the refresh token together with the access token from the authorization server
2. The access token should expire in a short time
3. Once the access token is expired, the client request to refresh the access token by sending the refresh token to the authorization server
4. The authorization server sends a new access token together with a new refresh token

## Token Validation approachs at the Resource Server
1. Allows the resource server to directly call the authorization server to verify an issued token
2. Blackboarding
    * authorization server stores tokens in the a common database, resource server fetch the token from this db
3. The authorization server signs the token when issuing it. The resource server validates the signature.
    * it generally uses JSON Web Tokens (JWTs)
