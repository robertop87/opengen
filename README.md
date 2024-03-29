# Update 5

The java client generator does not generate JsonNullable for the next library configurations: okhttp-gson, microprofile

But next libraries generate the JsonNullable as expected:

  jersey2, jersey3, feign, retrofit2, resttemplate, webclient, resteasy, vertx, google-api-client, rest-assured, native, apache-httpclient

Latest incorrect [Pet.java generated](build/generated-src/swagger/src/main/java/org/openapitools/client/model/Pet.java).

Expected model generated [Pet.java](src/test/resources/Pet.java)

# Update 4.

The java client generator works as expected using library set to webclient.

```groovy
tasks.register('generateClient', GenerateTask) {
    generatorName.set("java")
    library.set("webclient") // only with webclient the JsonNullable are generated properly
    inputSpec.set(swaggerFile.toString())
    outputDir.set(swaggerOutputDir)
    configOptions.set([
            dateLibrary: "java8",
            useJakartaEe: "true",
            openApiNullable: "true" // This is enabled by default
    ])
}
```

# Update 3.

Trying to enable `x-is-jackson-optional-nullable` extension. According docs it should generate the expected JsonNullable types.

But it's not working, the extension has no effect in swagger.yml.

# Update 2.

The update 1, its fine if the goal is generate a server application with spring.

Unfortunately generate a client still having the same issue reported.

Using generatorName: "spring" the Pet result is: [Pet.java](src/test/resources/Pet.java)

But using generatorName: "java" the Pet has no the JsonNullable types: [Pet.java](build/generated-src/swagger/src/main/java/org/openapitools/client/model/Pet.java)

# Update 1. (Nothing fixed)

The issue was fixed in this [commit](https://github.com/robertop87/opengen/commit/540a8dd4f6e51f4b95eff452f63e32a10dc50a48). The generate [Pet.java](src/test/resources/Pet.java) is correct.

You can read the above lines to understand the reported issue.


## Environment

Gradle plugin: "org.openapi.generator" version "7.3.0"
OpenApi File Version: "3.0.1"
Java 21

## Problem

The file [swagger.yml](swagger.yml) defines the tag property as nullable for Pet schema.

```yaml
    Pet:
      type: object
      required:
        - id
        - name
      properties:
        id:
          type: integer
          format: int64
        name:
          type: string
        tag:
          type: string
          nullable: true 
```

The generated [Pet.java](build/generated-src/swagger/src/main/java/org/openapitools/client/model/Pet.java) class has `tag` just as String type:

```java
private String tag;
```

And the expected type and the default value should be:

```java
private JsonNullable<String> tag = JsonNullable.undefined();
```

## Note

Run next command to generate Pet.java

```shell
./gradlew clean assemble
```
