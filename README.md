# GraphQL Spring Webclient
[![Maven Central](https://img.shields.io/maven-central/v/com.graphql-java-kickstart/graphql-webclient-spring-boot-starter.svg)](https://maven-badges.herokuapp.com/maven-central/com.graphql-java-kickstart/graphql-webclient-spring-boot-starter)
![Publish snapshot](https://github.com/graphql-java-kickstart/graphql-spring-webclient/workflows/Publish%20snapshot/badge.svg)
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=graphql-java-kickstart_graphql-spring-webclient&metric=alert_status)](https://sonarcloud.io/dashboard?id=graphql-java-kickstart_graphql-spring-webclient)
[![GitHub contributors](https://img.shields.io/github/contributors/graphql-java-kickstart/graphql-spring-webclient)](https://github.com/graphql-java-kickstart/graphql-spring-webclient/graphs/contributors)
[![Discuss on GitHub](https://img.shields.io/badge/GitHub-discuss-orange)](https://github.com/graphql-java-kickstart/graphql-spring-webclient/discussions)


Reactive GraphQL client for consuming GraphQL APIs from a Spring Boot application.
Provides OAuth2 authorization through configuration.

## Getting started

Add the starter to your project.

When using Maven:
```xml
<dependency>
  <groupId>com.graphql-java-kickstart</groupId>
  <artifactId>graphql-webclient-spring-boot-starter</artifactId>
  <version>1.0.0</version>
</dependency>
```

When using gradle:
```groovy
implementation "com.graphql-java-kickstart:graphql-webclient-spring-boot-starter:1.0.0"
```

Configure at least the URL of the GraphQL API to consume:
```yaml
graphql:
  client:
    url: https://graphql.github.com/graphql
```

The starter creates a Spring bean of type `GraphQLWebClient` that you can use in your
classes to send queries. A simplified example might look like this:

```java
@Component
class MyClass {
  
  private final GraphQLWebClient graphQLWebClient;
  
  MyClass(GraphQLWebClient graphQLWebClient) {
    this.graphQLWebClient = graphQLWebClient;
  }
  
  String helloWorld() {
    GraphQLRequest request = GraphQLRequest.builder().query("query { hello }").build();
    GraphQLResponse response = graphQLWebClient.post(request).block();
    return response.get("hello", String.class);
  }
}
```

### Using latest Snapshots

You can use the latest Snapshots by configuring the Snapshot repository, see https://www.graphql-java-kickstart.com/servlet/#using-the-latest-development-build.


## Configuration

The following tables list the configurable properties of the GraphQL Spring Webclient and their default values.
These properties are configured with the prefix `graphql.client`, e.g. the property listed in the table as `url` 
should be defined as `graphql.client.url` in your Spring Boot configuration files.

| Property | Description |
|----------|-------------|
| `url` | Full URL of the GraphQL API to connect to, e.g. https://graphql.github.com/graphql |
| `oauth2.client-id` | OAuth2 client id |
| `oauth2.client-secret` | OAuth2 client secret |
| `oauth2.token-uri` | Token URI of the identity provider |
| `oauth2.authorization-grant-type` | By default the grant type `client_credentials` is used |
 
### Max in memory size

In case you need to work with large responses you might run into the following error:
```
Exceeded limit on max bytes to buffer : 262144
```
In that case starting with version 0.1.2 you can use the default Spring Boot configuration property to configure
the max in memory size to use:
```properties
spring.codec.max-in-memory-size=10MB
``` 

```shell
./gradlew clean

./gradlew build

./gradlew :graphql-webclient:publishToMavenLocal 
```