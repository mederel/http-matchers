http-matchers
=============

A set of [hamcrest-matchers](http://hamcrest.org/JavaHamcrest/) to test 
your web services via the standard [Java API for RESTful Services (JAX-RS)](https://jax-rs-spec.java.net/)

This library is available at [Maven Central Repository](http://search.maven.org/).
Add this dependency to your `pom.xml`

    <dependency>
	    <groupId>org.valid4j</groupId>
	    <artifactId>http-matchers</artifactId>
	    <version>1.0</version>
    </dependency>

The [JAX-RS client API (javax.ws.rs.client)](https://docs.oracle.com/javaee/7/api/index.html?javax/ws/rs/client/package-summary.html)
can be downloaded from here, but is usually bundled with the client implementation you are using, e.g Jersey.

    <dependency>
	    <groupId>javax.ws.rs</groupId>
	    <artifactId>javax.ws.rs-api</artifactId>
	    <version>2.0.1</version>
    </dependency>

## Usage guide

Example usage:

    // Statically import the library entry point:
    import static org.valid4j.matchers.http.HttpResponseMatchers.*;
    
    // Invoke your web service using plain JAX-RS. E.g:
    Client client = ClientBuilder.newClient();
    Response response = client.target("http://example.org/hello").request("text/plain").get();
    
    // Verify the status code of response
    assertThat(response, hasStatusCode(Family.SUCCESSFUL));
    assertThat(response, hasStatusCode(200));
    assertThat(response, hasStatusCodeOf(Status.OK));

    // Verify status code and reason of response
    assertThat(response, hasStatus(Status.OK));
    assertThat(response, hasStatus(400, "Bad Request");
    
    // Verify representation metadata of the response
    assertThat(response, hasContentType(MediaType.APPLICATION_JSON_TYPE));
    assertThat(response, hasContentType(compatibleWith(MediaType.APPLICATION_JSON_TYPE)));
    assertThat(response, hasContentEncoding("gzip"));
    assertThat(response, hasContentLocation(URI.create("http://example.com/123")));
    assertThat(response, hasContentLength(lessThan(2345)));

    // Verify headers of response (by-existence, by-value, by-iterable)
    assertThat(response, hasHeader("Expires"));
    assertThat(response, hasHeader("Content-Encoding", equalTo("gzip")));
    assertThat(response, hasHeader("Age", equalTo(42)));
    assertThat(response, hasHeaderValues("Cache-Control",
                contains(equalTo("no-cache"), equalTo("no-store"))));

    // Verify the existence of a message body in the response
    assertThat(response, hasEntity());

    // Verify the message body content (aka entity) of the response
    // NOTE: This will buffer the entity so make sure to close the response
    assertThat(response, hasEntity(equalTo("content")));
    
    // Map the body to a specific Java type (using a MessageBodyReader)
    // E.g
    assertThat(response, hasEntity(String.class, equalTo("...")));
    assertThat(response, hasEntity(MyBody.class, myBodyMatcher)));

    // Verify language of response
    assertThat(response, ofLanguage("en-GB"));
    assertThat(response, ofLanguage(Locale.UK));
    assertThat(response, ofLanguage(equalTo(Locale.UK)));

    // Verify location of the response
    URI location = URI.create("http://example.com/123");
    assertThat(response, hasLocation(equalTo(location)));

    ...
    
## Project license

This software is licensed under [Apache Software License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0.txt)
