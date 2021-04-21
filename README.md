<img src="https://www.sipgatedesign.com/wp-content/uploads/wort-bildmarke_positiv_2x.jpg" alt="sipgate logo" title="sipgate" align="right" height="112" width="200"/>

# sipgate.io Java Personal Access Token example

To demonstrate how to authenticate against the sipgate REST API using personal access tokens,
we query the `/account` endpoint which provides basic account information.

For further information regarding sipgate REST API please visit https://api.sipgate.com/v2/doc.

For more information on how to create a token, visit https://www.sipgate.io/rest-api/authentication#personalAccessToken.

### Prerequisites

- JDK 8

### How To Use

Navigate to the project's root directory.

Change token id and token in PersonalAccessToken.java:

```java
String tokenId = "YOUR_SIPGATE_TOKEN_ID";
String token = "YOUR_SIPGATE_TOKEN";
```
The token should have the `account:read` scope.


Run the application:

```bash
./gradlew run
```

### How It Works

Request parameters like url and credentials are defined as follows:

```java
private static final String baseUrl = "https://api.sipgate.com/v2";

...

String credentials = tokenId + ":" + token;

String base64EncodedCredentials = Base64.getEncoder().encodeToString(credentials.getBytes());
```

**Note**: Basic Auth requires the credentials to be Base64-encoded.

We use Unirest for request generation and execution.
The request URL consists of the base url defined above and the endpoint `/account`.
The value of the Authorization header consists of the keyword `Basic` followed by a space and the Base64-encoded credentials.
This example prints the HTTP status code and body of the response to the console.

```java
HttpResponse<String> response = Unirest.get(baseUrl + "/account")
	.header("Authorization", "Basic " + base64EncodedCredentials)
	.asString();

	System.out.println("Status: " + response.getStatus());
	System.out.println("Body: " + response.getBody());
```

> If OAuth should be used for authorization instead of Basic Auth we do not use the `.basicAuth(tokenId, token)` method. Instead we set the `Authorization` header to `Bearer` followed by a space and the access token: `.header("Authorization", "Bearer " + accessToken)`. For an example application interacting with the sipgate API using OAuth see our [sipgate.io Java Oauth example](https://github.com/sipgate-io/sipgateio-oauth-java).

### Common Issues

#### HTTP Errors

| reason                          | errorcode |
| ------------------------------- | :-------: |
| token id and/or token are wrong |    401    |
| credentials not Base64-encoded  |    401    |
| wrong REST API endpoint         |    404    |
| wrong request method            |    405    |

### Related

- [Unirest documentation](http://unirest.io/)

### Contact Us

Please let us know how we can improve this example.
If you have a specific feature request or found a bug, please use **Issues** or fork this repository and send a **pull request** with your improvements.

### License

This project is licensed under **The Unlicense** (see [LICENSE file](./LICENSE)).

### External Libraries

This code uses the following external libraries

- unirest:  
   Licensed under the [MIT License](https://opensource.org/licenses/MIT)  
   Website: http://unirest.io/java

---

[sipgate.io](https://www.sipgate.io) | [@sipgateio](https://twitter.com/sipgateio) | [API-doc](https://api.sipgate.com/v2/doc)
