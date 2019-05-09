<img src="https://www.sipgatedesign.com/wp-content/uploads/wort-bildmarke_positiv_2x.jpg" alt="sipgate logo" title="sipgate" align="right" height="112" width="200"/>


# sipgate.io Java Basic Auth example

To demonstrate how to authenticate against the sipgate REST API using HTTP Basic Auth, 
we query the `/account` endpoint which provides basic account information.

For further information regarding sipgate REST API please visit https://api.sipgate.com/v2/doc


### Prerequisites
+ JDK 8
+ Maven


### How To Use
Navigate to the project's root directory.

Install dependencies manually or use your IDE's import functionality:
```bash
$ mvn dependency:resolve
```

Change username and password in BasicAuthExample.java:
```java
String username = "YOUR_SIPGATE_EMAIL";
String password = "YOUR_SIPGATE_PASSWORD";
```

Build JAR
```bash
$ mvn package
```

Run the application:
```bash
$ java -jar target/sipgateio-basicauth-java-1.0-SNAPSHOT-jar-with-dependencies.jar
```


### How It Works
Request parameters like url and credentials are defined as follows:
```java
private static final String baseUrl = "https://api.sipgate.com/v2";

...

String credentials = username + ":" + password;

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

> If OAuth should be used for authorization instead of Basic Auth we do not use the `.basicAuth(username, password)` mehthod. Instead we set the `Authorization` header to `Bearer` followed by a space and the access token: `.header("Authorization", "Bearer " + accessToken)`. For an example application interacting with the sipgate API using OAuth see our [sipgate.io Java Oauth example](https://github.com/sipgate-io/sipgateio-oauth-java).


### Basic Auth
Basic access authentication (Basic Auth) is an easy to use, well known, and well supported authentication method. 
You can find a lot of documentation about this authentication method on the internet, e.g: [Wikipedia](https://en.wikipedia.org/wiki/Basic_access_authentication) or [RFC 2617](https://www.ietf.org/rfc/rfc2617.txt).

To use Basic Auth, you simply have to provide an Authorization header with each request. 
The header uses the keyword `Basic` followed by a space and a credentials string. 
The credentials string is `username:password` Base64-encoded.

For example, if your username is `John` and your password is `topsecret`, 
your plaintext credentials string would be `John:topsecret`. 
The Base64-encoded string would be `Sm9objp0b3BzZWNyZXQ=`.

The complete header would look like:

`Authorization: Basic Sm9objp0b3BzZWNyZXQ=`


### Common Issues

#### HTTP Errors
| reason | errorcode | 
| ------------- |:-------------:|
| username and/or password are wrong | 401 |
| credentials not Base64-encoded | 401 |
| wrong REST API endpoint | 404 |
| wrong request method | 405 |


### Related

+ [Unirest documentation](http://unirest.io/)


### Contact Us
Please let us know how we can improve this example. 
If you have a specific feature request or found a bug, please use **Issues** or fork this repository and send a **pull request** with your improvements.


### License
This project is licensed under **The Unlicense** (see [LICENSE file](./LICENSE)).


### External Libraries
This code uses the following external libraries

+ unirest:  
    Licensed under the [MIT License](https://opensource.org/licenses/MIT)  
    Website: http://unirest.io/java


----

[sipgate.io](https://www.sipgate.io) | [@sipgateio](https://twitter.com/sipgateio) | [API-doc](https://api.sipgate.com/v2/doc)
