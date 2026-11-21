# orders-api

Enterprise Quarkus 3 / Java 21 microservice with Ambient Mesh, OpenAPI, and Command Centre telemetry

Catalog: **System** `mallamsbank` ← **Component** (service) `orders-api` **providesApis** `orders-api-api`.

| Piece | Path |
|---|---|
| App | `GET /health`, `GET /api` |
| Tests | `mvn test` |
| Sonar | `sonar-project.properties` |
| CI | `Jenkinsfile` |
| Deploy | `chart/` (Helm + OSSM: Route → Gateway → HTTPRoute, deny-all, STRICT mTLS) |
| IDE | `devfile.yaml` |

```bash
mvn -q test
helm lint chart
```
