# JWT

JSON web tokens are the standard was for transferring data (as a JSON object) securely between two systems. This information issecure because it can be digitially signed, either with an HMAC algorithm (encryption) or a public/private key pair. The benefits of HMAC is that it provides secrecy between both parties (good for password handling). State is held in the client-side session rather than on the server. 

### When to Use JWT

- Authorization
  - When a user is authenticated and logged in, each request they make will include JWT, allowing them to access routes, services, and resources as permitted with the token provided. 
- Information Exchange
  - Securely sharing information between parties. JWT can be signed, meaning with the use of public/private keys, you can verify the senders are correct and that the data hasn't been messed with.

### Structure 

- Header

  - typically has two parts
    - type of token (JWT)
    - signing algorithm to be used (HMAC, SHA256, RSA)
  - Looks like this:

  ``` json
  {
  	"alg": "HMAC",
  	"typ": "JWT"
  }
  ```

- Payload

  - Contains the claims

    - info about the entity (usually the user)
    - There are three types of claims:
      - Registered
        - Predefined
        - not mandatory (but recommended)
        - Provide interoperable claims (issuer, expiration time, subject, audience and others)
      - Public
        - Defined at will
          - Should use IANA JSON web token registry or defined as an URI
      - Private Claims
        - Custom claims to share info that parties agree on

  - An example payload may look like this:

    - ```json
      {
      	"sub": "1234567890",
      	"name": "John Mar",
      	"admin": true
      }
      ```

  - IMPORTANT

    - Signed tokens are readable by anyone, so dont provide private information in the payload or header unless it is encrypted

- Signature

  - Created with the encoded header, encoded payload, a secret, the algorithm specified in the header, and sign

  - It is used to verify the massage wasnt changed along the way and can verify the sender is who they say they are

  - Might look like this: 

    ``` 
    HMACSHA256(
    	base64UrlEncide(header) + "." +
    	base64UrlEncode(payload),
    	secret
    	)
    ```

The complete syntax will look like this:

``` 
HEADER + '.' + PAYLOAD + '.' + SIGNATURE
```



To practice with creating tokens, you can visit: https://jwt.io/#debugger-io 

### Workflow

After a successful login, a token will be provided to the client. In general, tokens should not be held onto longer than necessary. Because different user may have access to the same machine, it is recommened not to store tokens (or sensitive session data) with local storage, as all local users will have access. Instead, use session storage. 

When a user wants to access a protected route or resource, they should send then JWT, in the Authorization header, using the Bearer schema:

``` 
Authorization: Bearer <token>
```

The server will verify the token and then allow access if authorized. 









### Resources

- https://jwt.io/introduction/
- https://medium.com/javascript-in-plain-english/jwt-auth-with-node-and-passport-js-c41a91d333e0