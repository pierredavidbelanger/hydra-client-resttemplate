# hydra-client-resttemplate

This is a swagger-codegen generated REST client to access [Hydra](https://github.com/ory/hydra) API

## Usage

Add a dependency to your project's `pom.xml` 

```xml
<dependency>
    <groupId>ca.pjer</groupId>
    <artifactId>hydra-client-resttemplate</artifactId>
    <version>1.0.2-beta.9-8555973</version>
</dependency>
```

Add a package to be scanned to your `@SpringBootApplication` and define a `@Bean` to customize the `URI` and the `RestTemplate` of the `ApiClient`:

```java
package mypackage;

import ca.pjer.hydra.client.ApiClient;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.client.RestTemplateBuilder;
import org.springframework.context.annotation.Bean;

@SpringBootApplication(scanBasePackageClasses = {
        mypackage.App.class,
        ca.pjer.hydra.client.api.OAuth2Api.class
})
public class App {

    @Value("${hydra.api.uri:http://hydra.lvh.me:4445}")
    String hydraApiUri;

    @Bean
    ApiClient apiClient(RestTemplateBuilder restTemplateBuilder) {
        var bean = new ApiClient(restTemplateBuilder.build());
        bean.setBasePath(hydraApiUri);
        return bean;
    }

    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }
}
```

Then you are ready to inject any of the components in the `ca.pjer.hydra.client.api` package:

```java
    @Autowired
    protected OAuth2Api oAuth2Api;
```
