# Mock Endpoints Spring Boot Starter

> A Spring Boot starter that exposes mock endpoints from your existing controller contracts so frontend teams can start before backend logic is finished.

## What it does

If your controller already exists, this library can register a mock version of that endpoint and return fake data shaped like your real response DTOs.

Example:

```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @GetMapping
    public ResponseEntity<List<UserResponse>> findAll() {
        throw new UnsupportedOperationException("Not implemented yet");
    }
}
```

The library can expose:

```text
GET /api/users/test
```

and return generated fake JSON immediately.

## Maven dependency

```xml
<dependency>
    <groupId>io.github.waelhasnaoui1</groupId>
    <artifactId>mock-endpoints-spring-boot-starter</artifactId>
    <version>1.0.1</version>
</dependency>
```

Maven Central:

[https://repo1.maven.org/maven2/io/github/waelhasnaoui1/mock-endpoints-spring-boot-starter/1.0.1/](https://repo1.maven.org/maven2/io/github/waelhasnaoui1/mock-endpoints-spring-boot-starter/1.0.1/)

## Quick configuration

Example `application.yml`:

```yaml
mock:
  endpoints:
    enabled: true
    suffix: /test
    base-package: com.example.demo
    default-list-size: 5
    seed: 42
    response-delay-ms: 0
    max-depth: 3
    inspection:
      enabled: true
      path: /mock-endpoints
    scenarios:
      enabled: true
      header-name: X-Mock-Scenario
      large-list-size: 20
      error:
        status: 503
        body: '{"error":"mock error scenario"}'
      flaky:
        enabled: true
        failure-rate: 0.35
      timeout:
        status: 504
        delay-ms: 3000
        body: '{"error":"mock timeout scenario"}'
```

## Main features

- Auto-registers mock routes from Spring MVC controllers
- Generates fake responses from return types
- Supports `List`, `Set`, `Map`, `Optional`, and `Page`
- Supports records, nested DTOs, generic wrappers, path variables, and `PATCH`
- Supports fixed JSON responses and per-endpoint overrides
- Supports named scenarios like `empty`, `error`, `large-list`, `timeout`, and `flaky`
- Supports OpenAPI-based mock route generation
- Exposes an inspection endpoint to view registered mocks

## Scenario examples

```text
GET /api/users/test?scenario=empty
GET /api/users/test?scenario=large-list
GET /api/users/test?scenario=error
GET /api/users/test?scenario=timeout
```

You can also select scenarios with a header:

```text
X-Mock-Scenario: empty
```

## Useful endpoints

```text
GET /mock-endpoints
```

This returns the registered mock routes and available scenario information.

## Annotations

The library includes field-level annotations for more realistic fake values:

- `@FakeName`
- `@FakeEmail`
- `@FakeNumber`
- `@FakeDate`
- `@FakePhone`
- `@FakeAddress`
- `@FakeCompany`
- `@FakeUrl`

It also supports endpoint-level customization with:

- `@MockEndpoint`
- `@MockScenario`

## Example endpoint override

```java
@GetMapping("/catalog")
@MockEndpoint(
    status = 202,
    headers = {"X-Mock-Source=resource"},
    bodyResource = "mock/catalog-response.json",
    delayMs = 250
)
public ResponseEntity<Map<String, Object>> catalog() {
    throw new UnsupportedOperationException("Not implemented yet");
}
```

## OpenAPI support

You can also register mock endpoints from an OpenAPI spec:

```yaml
mock:
  endpoints:
    openapi:
      enabled: true
      spec-location: classpath:openapi/test-api.yaml
      use-examples: true
```



## Use case

This starter is useful when:

- frontend developers need an API before backend logic is ready
- backend and frontend teams want to work in parallel
- you already have controller contracts and DTOs
- you want fast internal mocks without creating a separate mock server

## Feedback

If you test it in a real Spring Boot project, feedback is very welcome:

- what worked well
- what broke
- what felt confusing
- what feature should come next
