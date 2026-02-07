# xis-prices-api

Library to generate Java server-side code from an OpenAPI specification.

This project is not a microservice; it is a generator library that produces Spring-based API interfaces and models from
an OpenAPI YAML file. The generated sources are intended to be used by other modules or applications.

## Overview

- Module: `xis-prices-api-server` (part of the `prices-api` multi-module project).
- Purpose: Generate Java server API interfaces and models from the OpenAPI spec located at `api/openapi.yaml`.
- Generator: OpenAPI Generator (Maven plugin configured in `xis-prices-api-server/pom.xml`).

## Key configuration (from `xis-prices-api-server/pom.xml`)

- Java source/target: 25
- OpenAPI Generator Maven Plugin version: 7.19.0
- Generator: `spring`
- API package: `com.xis.prices.api`
- Model package: `com.xis.prices.model`
- Supporting file generated: `ApiUtil.java`
- Plugin config options used:
    - `useSpringBoot3=true`
    - `interfaceOnly=true`
    - `useTags=true`
    - `reactive=true`
    - `useVirtualThreads=true`

Dependencies declared in the module:

- `org.springdoc:springdoc-openapi-starter-webflux-ui`
- `org.openapitools:jackson-databind-nullable`

## Prerequisites

- JDK 25
- Maven 3.8+ (or compatible)
- Ensure the OpenAPI spec is present at `api/openapi.yaml` relative to the repository root.

## Generate sources

The `openapi-generator-maven-plugin` is configured in the `xis-prices-api-server` module's `pom.xml`. There are two
common ways to run it:

1) Run the plugin goal explicitly (useful if execution isn't bound to a lifecycle phase):

```powershell
mvn org.openapitools:openapi-generator-maven-plugin:7.19.0:generate `
  -DinputSpec=api/openapi.yaml `
  -DgeneratorName=spring `
  -DapiPackage=com.xis.prices.api `
  -DmodelPackage=com.xis.prices.model `
  -DconfigOptions.useSpringBoot3=true `
  -DconfigOptions.interfaceOnly=true `
  -DconfigOptions.useTags=true `
  -DconfigOptions.reactive=true `
  -DconfigOptions.useVirtualThreads=true
```

2) Use the Maven lifecycle if the plugin execution is bound (common in this project):

```powershell
cd xis-prices-api-server; mvn clean generate-sources
```

Generated sources are typically placed under `target/generated-sources/openapi` (verify plugin configuration if
different).

## Typical workflow

1. Update `api/openapi.yaml`.
2. Run the generator (see commands above).
3. Review the generated sources under `xis-prices-api-server/target/generated-sources`.
4. Implement application logic by providing implementations for the generated interfaces in your consuming module or
   application.

## Notes and recommendations

- This module produces code artifacts to be used by other modules or applications — it is not a runnable microservice by
  itself.
- The generator options enable Spring Boot 3 and reactive APIs; change plugin config if you need non-reactive or
  different server code.
- Treat generated code as produced artifacts: prefer not to manually edit generated files unless you copy them into a
  handwritten source tree. Keep the OpenAPI spec and generator config in sync to avoid drift.
- If you want to change generation details, edit `xis-prices-api-server/pom.xml` plugin configuration or pass different
  `-D` parameters when invoking the plugin.

## Where to look next

- OpenAPI spec: `api/openapi.yaml`
- Module pom: `xis-prices-api-server/pom.xml`
- Previously generated artifacts (example): `xis-prices-api-server/target/classes/com/xis/prices/api` and
  `xis-prices-api-server/target/generated-sources/openapi`
