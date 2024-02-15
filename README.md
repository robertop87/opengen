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
