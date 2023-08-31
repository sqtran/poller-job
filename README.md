# k8s-job

This is a sample project that demonstrates the use of the Fabric8 library to programatically create Kubernetes objects. Specifically, it creates a Kubernetes Job with the Fabric8 DSL.

This project uses Quarkus, the Supersonic Subatomic Java Framework.

If you want to learn more about Quarkus, please visit its website: https://quarkus.io/ .

## Running the application in dev mode

You can run your application in dev mode that enables live coding using:
```shell script
./mvnw compile quarkus:dev
```

> **_NOTE:_**  Quarkus now ships with a Dev UI, which is available in dev mode only at http://localhost:8080/q/dev/.

## Packaging and running the application

The application can be packaged using:
```shell script
./mvnw package
```
It produces the `quarkus-run.jar` file in the `target/quarkus-app/` directory.
Be aware that it’s not an _über-jar_ as the dependencies are copied into the `target/quarkus-app/lib/` directory.

The application is now runnable using `java -jar target/quarkus-app/quarkus-run.jar`.

If you want to build an _über-jar_, execute the following command:
```shell script
./mvnw package -Dquarkus.package.type=uber-jar
```

The application, packaged as an _über-jar_, is now runnable using `java -jar target/*-runner.jar`.

## Creating a native executable

You can create a native executable using:
```shell script
./mvnw package -Pnative
```

Or, if you don't have GraalVM installed, you can run the native executable build in a container using:
```shell script
./mvnw package -Pnative -Dquarkus.native.container-build=true
```

You can then execute your native executable with: `./target/k8s-job-1.0.0-SNAPSHOT-runner`

If you want to learn more about building native executables, please consult https://quarkus.io/guides/maven-tooling.

## Related Guides

- Kubernetes ([guide](https://quarkus.io/guides/kubernetes)): Generate Kubernetes resources from annotations




## Important

Make sure your service account has access to create Jobs.  It needs a RoleBinding for API group "batch" in the namespace.

You'll see an error like the following if you don't have permissions.

```text
io.fabric8.kubernetes.client.KubernetesClientException: Failure executing: POST at: https://172.30.0.1:443/apis/batch/v1/namespaces/steve-dev/jobs. Message: Forbidden!Configured service account doesn't have access. Service account may have been revoked. jobs.batch is forbidden: User "system:serviceaccount:steve-dev:default" cannot create resource "jobs" in API group "batch" in the namespace "steve-dev".
```

You'll get similar errors reading the ConfigMaps.  Here's a sample RoleBinding snippet.

```
rules:
  - verbs:
      - get
      - watch
      - list
      - create
    apiGroups:
      - batch
    resources:
      - jobs
  - verbs:
      - get
      - list
    apiGroups:
      - ''
    resources:
      - configmaps
```