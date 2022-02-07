---
title: Spring Framework - Swagger 적용
author: SeungEun Baek
date: 2022-02-07 20:18 
description: Spring MVC Swagger 적용 방법 작성
category: bse
layout: post
---
[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fdev-seungeun.github.io%2F1study%2Fswagger_ui&count_bg=%23FEC8E6&title_bg=%23B2ADAD&icon=&icon_color=%23515050&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)   

## Swagger 2.9.2 적용

> JAVA8 이상 지원   


### 1. pom.xml 추가   
```xml  
<!-- swagger 2.9.2 -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
```

### 2. SwaggerConfig.java 파일 생성   
> build시 유의사항
> @Profile("!prod") : 운영서버에 Swagger접근을 막아야 할 경우 Profile 설정이 가능하다.

```java
@Profile("!prod")
@Configuration
@EnableSwagger2  
public class SwaggerConfig{

    @Autowired
    ApplicationContext ctx;

    @Bean
    public Docket customImplementation() {
		
        List<Parameter> global = new ArrayList<>();
        global.add(new ParameterBuilder().name("header1").description("헤더1")
                 .parameterType("header").required(true).modelRef(new ModelRef("string")).defaultValue("디폴트값").build());
        global.add(new ParameterBuilder().name("header2").description("헤더2")
                 .parameterType("header").required(true).modelRef(new ModelRef("string")).defaultValue("디폴트값").build());

        List<ResponseMessage> responseMessages = new ArrayList<>(); 
        responseMessages.add(new ResponseMessageBuilder().code(204).message("응답 콘텐츠 없음").build());
        responseMessages.add(new ResponseMessageBuilder().code(500).message("서버 내부 오류").build());

        // Protocol 또는 baseUrl 변경이 필요 할 경우 적용
        // Set<String> protocol = new HashSet<>(Arrays.asList("https","http")); 
        // String baseUrl = "DEV".equals(ConstantsManager.SERVER.toUpperCase()) ? "dev-url" : "prod-url"; 

        return new Docket(DocumentationType.SWAGGER_2).apiInfo(getApiInfo()) 
						      .globalResponseMessage(RequestMethod.GET, responseMessages)
						      .globalOperationParameters(global)
						      .useDefaultResponseMessages(false)
						      //.protocols(protocol)
						      //.host(baseUrl)
						      //.securityContexts(Arrays.asList(securityContext()))
						      //.securitySchemes(Arrays.asList(apiKey()))
						      .select()     
						      .apis(RequestHandlerSelectors.basePackage("com.my.package"))
						      .paths(PathSelectors.ant("/**")) 
						      .build(); 
    } 

    private ApiKey apiKey() {
        return new ApiKey("Auth-Token", "header");
    }
	
    private SecurityContext securityContext() {
        return springfox.documentation
                        .spi.service
                        .contexts
                        .SecurityContext
                        .builder()
                        .securityReferences(defaultAuth()).forPaths(PathSelectors.any()).build();
    }
    
    List<SecurityReference> defaultAuth() {
        AuthorizationScope authorizationScope = new AuthorizationScope("global", "accessEverything");
        AuthorizationScope[] authorizationScopes = new AuthorizationScope[1];
        authorizationScopes[0] = authorizationScope;
        return Arrays.asList(new SecurityReference("Auth-Token", authorizationScopes));
    }
    
    private ApiInfo getApiInfo() {
        return new ApiInfoBuilder().title("REST API")
                                   .version("v1.0")
                                   .build();
    }
	  
}
```

### 3. AppInitializer.java 수정
```java
@Override
protected Class<?>[] getServletConfigClasses() {
    // TODO Auto-generated method stub
    return new Class<?>[] { WebMvcConfig.class, SwaggerConfig.class };
}
```

### 4. WebMvcConfig.java 수정 (아래 코드 추가)
```java
@Override
public void addResourceHandlers(ResourceHandlerRegistry registry) {
    registry.addResourceHandler("/swagger-ui.html**").addResourceLocations("classpath:/META-INF/resources/swagger-ui.html");
    registry.addResourceHandler("/webjars/**").addResourceLocations("classpath:/META-INF/resources/webjars/");
}
```

### 5. Swagger Annotation 설정   
```java
@ApiOperation(value = "", notes = "API1 정보 조회", httpMethod = "GET")
@ApiImplicitParams({@ApiImplicitParam(name="CODE", value="코드 / ex) code1", paramType="path", required=true),
                    @ApiImplicitParam(name="NAME", value="이름 / ex) Karen", paramType="query", required=true)})
@RequestMapping(value = "/api/code/{CODE}/info", method = RequestMethod.GET, produces = MediaType.APPLICATION_JSON_UTF8_VALUE)
public ResponseEntity<Map<String, Object>> apiMethod1(HttpServletRequest request);
```
<br>   

> #### 적용된 모습 - Swagger UI
![image](https://user-images.githubusercontent.com/80504390/152713975-f57e02f2-a8c4-44c7-8200-2b0f7f19a3dc.png)
<br><br>   

> #### Swagger-UI에 노출하지 않을 Method : 각 메서드에 @ApiIgnore 설정
```java
@ApiIgnore
@RequestMapping(value = "/api/code/{CODE}/info", method = RequestMethod.GET, produces = MediaType.APPLICATION_JSON_UTF8_VALUE)
public ResponseEntity<Map<String, Object>> apiMethod1(HttpServletRequest request);
```
<br>   

> #### Swagger-UI에 Class전체를 노출하지 않을 경우 : Controller에 @ApiIgnore 설정
```java
@ApiIgnore
@RestController
public class TestController {
    ...
}
```

<br><br>

## Swagger 1.5.8 적용

> JAVA6, JAVA7 지원   


### 1. pom.xml 추가
```xml
<!-- jersey -->
<dependency>
    <groupId>io.swagger</groupId>
    <artifactId>swagger-jersey2-jaxrs</artifactId>
    <version>1.5.8</version>
</dependency> 
```

### 2. web.xml 추가
```xml
<servlet>
    <servlet-name>myServlet</servlet-name>
    <servlet-class>org.glassfish.jersey.servlet.ServletContainer</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      	<param-value> 
	    classpath:/META-INF/spring/spring-servlet-context.xml 
   	</param-value>
    </init-param>
    <load-on-startup>2</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>myServlet</servlet-name>
    <url-pattern>/api/*</url-pattern>
</servlet-mapping>
<servlet>
    <servlet-name>SwaggerConfig</servlet-name>
    <servlet-class>com.my.package.config.SwaggerConfig</servlet-class>
    <load-on-startup>3</load-on-startup>
</servlet>
```

### 3. SwaggerConfig.java 파일 생성
```java
import io.swagger.jaxrs.config.SwaggerContextService;
import io.swagger.models.Info;
import io.swagger.models.Swagger;
import io.swagger.models.Tag;

import javax.servlet.ServletConfig;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;

public class Bootstrap extends HttpServlet {

    private static final long serialVersionUID = 8991662522301438970L;

    @Override
    public void init(ServletConfig config) throws ServletException {

        super.init(config);
         
        BeanConfig beanConfig = new BeanConfig(); 
        beanConfig.setTitle("REST API"); 
        beanConfig.setDescription("REST API 입니다."); 
        beanConfig.setVersion("1.0");
        //beanConfig.setSchemes(new String[]{"http"});
        //beanConfig.setHost("localhost:8080"); 
        beanConfig.setBasePath("/api");
        beanConfig.setResourcePackage("com.my.package");
        beanConfig.setScan(true);
        
    }
}
```

### 4. Controller.java 수정
```java
@Component
@Path("/api")
@Api(value = "API")
@Produces({ MediaType.APPLICATION_JSON })
@Controller
public class TestController {

    @GET
    @Path("/api/{code}/info")
    @Produces({ MediaType.APPLICATION_JSON })
    @ApiOperation(value = "API조회", notes = "조회합니다.", tags = {"TEST"}, response = String.class)
    public Response getEmailInfo(@Context HttpServletRequest request,
                                 @HeaderParam("Accept") @DefaultValue("application/json") String accept,
                                 @ApiParam(value = "코드", required = true) @PathParam("code") String code) {             
                                 
        ...
        
    }
    
    ...
    
}
```

