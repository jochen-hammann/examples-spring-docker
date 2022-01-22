# Spring Boot Docker support

This example shows the build of Docker images with the aid of the Spring Boot Maven plugin.

### Docker (en)

#### Local Docker registry

Own Docker images are stored in a local Docker registry. As required by Docker, the connection to its registries must be secured by TLS. To connect to the local Docker registry, the respective Root CA certificate has to be installed at the host, which runs docker.

The following links contain information on how to deploy Docker images to a local registry:

* [Deploy a registry server](https://docs.docker.com/registry/deploying/)
* [Copy an image from Docker Hub to your registry](https://docs.docker.com/registry/deploying/#copy-an-image-from-docker-hub-to-your-registry)
* [How to use your own Registry](https://www.docker.com/blog/how-to-use-your-own-registry/)

<br/>

#### Docker login

If not already logged in to the local Nexus' Docker registry, the following CLI command has to be performed.

    docker login <host:port>

<br/>

#### Spring Boot Maven Plugin

To build a Docker image and push it to a (private) Docker registry, the Spring Boot Maven Plugin requires the following configuration.

```xml
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
    <configuration>
        <image>
            <name>${nexus.repository}/examples/docker-maven:dev</name>
        </image>
        <docker>
            <publishRegistry>
                <username>${nexus.user}</username>
                <password>${nexus.password}</password>
                <url>https://${nexus.repository}</url>
                <email>maven@host.com</email>
            </publishRegistry>
        </docker>
    </configuration>
</plugin>
```

<br/>

#### Build Docker image

To build the Docker image, the Spring Boot Maven Plugin may be used.

    mvn spring-boot:build-image -DskipTests

<br/>

#### Build and push Docker image

To build the Docker image, the Spring Boot Maven Plugin may be used.

    mvn spring-boot:build-image -Dspring-boot.build-image.publish=true -Dnexus.user=<username> -Dnexus.password=<password> -Dnexus.repository=<host:port> -DskipTests

<br/>

### See also

* [Spring Boot Maven Plugin: Packaging OCI Images](https://docs.spring.io/spring-boot/docs/current/maven-plugin/reference/htmlsingle/#build-image)
* [Spring Guides: Spring Boot with Docker](https://spring.io/guides/gs/spring-boot-docker/)
* [Spring Guides: Spring Boot Docker](https://spring.io/guides/topicals/spring-boot-docker)
* [Spring Blog: Creating Docker images with Spring Boot 2.3.0.M1](https://spring.io/blog/2020/01/27/creating-docker-images-with-spring-boot-2-3-0-m1)
* [Baeldung: Creating Docker Images with Spring Boot](https://www.baeldung.com/spring-boot-docker-images)
* [Baeldung: Dockerizing a Spring Boot Application](https://www.baeldung.com/dockerizing-spring-boot-application)
