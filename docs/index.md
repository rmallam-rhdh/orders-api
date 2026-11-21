# orders-api (`APP-1042`)

Enterprise Quarkus 3 / Java 21 microservice with Ambient Mesh, OpenAPI, and Command Centre telemetry

| Attribute | Value |
| :--- | :--- |
| **Application ID (`APPID`)** | `APP-1042` |
| **Owning Squad (`groupName`)** | `group:default/developers` |
| **System** | `mallamsbank` |
| **Provided OpenAPI Spec** | `orders-api-api` (`/openapi.yaml`) |
| **Dev Spaces User Namespace** | `bob-devspaces-q9m4x` |
| **Multi-Cluster Kubernetes ID** | `backstage.io/kubernetes-id: orders-api` |
| **Vault Enterprise Namespace** | `payments/nonprod` (`ledger-service`) |

---

## 1. Developer Workspace (OpenShift Dev Spaces)

Your isolated Dev Spaces namespace **`bob-devspaces-q9m4x`** is managed via GitOps in `charts/devspaces-user-namespaces/values.yaml` and reconciled by Argo CD (`devspaces-user-namespaces`).

* **Open Workspace directly in Dev Spaces:**
  [https://devspaces.apps.rosa.rosa-rwx5g.f8y9.p3.openshiftapps.com/dashboard/#/ide/bob-devspaces-q9m4x/orders-api](https://devspaces.apps.rosa.rosa-rwx5g.f8y9.p3.openshiftapps.com/dashboard/#/ide/bob-devspaces-q9m4x/orders-api)
* **Launch via Dev Spaces Factory URL:**
  [https://devspaces.apps.rosa.rosa-rwx5g.f8y9.p3.openshiftapps.com#https://github.com/rmallam-rhdh/orders-api?new&devfilePath=devfile.yaml&namespace=bob-devspaces-q9m4x](https://devspaces.apps.rosa.rosa-rwx5g.f8y9.p3.openshiftapps.com#https://github.com/rmallam-rhdh/orders-api?new&devfilePath=devfile.yaml&namespace=bob-devspaces-q9m4x)

---

## 2. CI/CD & Quality Gate (`Jenkinsfile` + SonarQube)

Jenkinsfile stages: **Unit Tests (JUnit 5 / Java 21) -> SonarQube Quality Gate -> Helm Lint & Template**.

* **Jenkins Pipeline Job:** `demo/orders-api`
* **SonarQube Project Key:** `demo-plugin-kitchen-sink` (`http://sonarqube.sonarqube.svc:9000`)

---

## 3. Multi-Cluster Runtime & Ambient Service Mesh (`/chart`)

North-south traffic flows through **OpenShift Route -> Kubernetes Gateway API (`Gateway`) -> `HTTPRoute` -> Service**.
* **Ambient Mesh Enrollment:** `istio.io/dataplane-mode=ambient` with `PeerAuthentication` (`STRICT` mTLS) and `AuthorizationPolicy` (deny-all + explicit ingress gateway allow).
* **Multi-Cluster Discovery:** Resources carry `backstage.io/kubernetes-id=orders-api` and `platform.idp/app-id=APP-1042`. Because `backstage.io/kubernetes-namespace` is omitted in `catalog-info.yaml`, RHDH discovers deployments and pods across **EDEV, NPRD, and PROD clusters** regardless of per-environment namespace naming.
