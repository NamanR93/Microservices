## Microservice:

Monolith is nothing but large application. The entire application have one large database.
It is very diff to release. The typical challenges with the monoliths are 

        - deployment complexity
        - Tightly coupled components causing longer release cycles 
        - Stability limitations
        - Technology lock-in : entire app build on single tech stack, adopting new is challenging.


## Microservices: 

- microserve architectural service style is an approach to developing a single application as a suite of small services, each running is own process and use diff data storage technology.

       1. It should be build on REST, should be following Rest API standards and best practices.
       2. Small well chosen deployable units.
       3. Dynamic scaling.

## Key micro services solutions:

           1. Spring boot.  : enables rapid development of REST API
           2. Spring Cloud  : provides number of solutions with respect to implementing your micro services architecture.
           3. Docker        : provides the deployment approach for all the micro services.
           4. Kubernetes    : manages the release, load balancing, service discovery.


￼
- First we will create hard coded Rest API, written in hard coded data.
- And then we will enhance it up from configuration and after that, we would connect it to decentralized configuration.


1.
        - In this we will create the LimitsController which return max, min limits.
        - Then we create configuration class to return those values.

2.
        - Its time to create spring cloud Config Server.

3.      - Set up git repo, initialise it, and create limits-service properties file.
        - add the properties from limits-service application.properties file to this repo.

4.     Connect spring cloud config server to local git repo. 
        - copy the git repo path and paste it in application properties-file
        -  Also add  @EnableConfigServer annotation 

5.     Connect limits-service to spring cloud config server
         - just mention the application.name = limits-service ( whatever the file name in git repo )
         - Now start the both server and hit 8080/limits

         - Now suppose we have diff profiles like qa, dev. For that create those files at git repo and hit /8888/limits-service/dev or /qa. ( in this way spring cloud server talks to git)
         - We want our application (limits-service) takes the values from spring-cloud server,  add profiles at application.prop files.
        
![alt text](Database.png)

         1.  First we will create the spring-exchange-service.
         2.  In this we enhance our microservice to able to return the env details, so when we call the currency-exchange service from the currency conversion micro service, so will know which instance of currency exchange micro service is actually responding back.

                   - setting up the dynamic port for it, using Environment.
                   - configure h2, JPA, create Repository, so that api can connect with db.

          3. Now create Currency conversion service
          4. Let’s see how to call currency-exchange from  currency-conversion micro service.

                  - for that we will make use of RestTemplate which is used to make rest API calls.

	    5.  #Feign
                -  Now We will use FEIGN , for service invocation.
                -  add dependency openFeign, with @EnableFeignClients to main file.
                -  for that will create CurrencyExchange Proxy Interface in conversion-service.
                -  Edit the controller and that’s it.

	    6.  #NamingServer or Service Registry
		        -  What if micro service wants to talk to another service, then we need naming server for which it can distribute the load and hit it, whenever it comes up.
		        -  We will setup first naming server, for that we use EUREKA naming server,  add dependency ,use @Eureka.. annotation  in main file
		        -  Let’s connect microservices to our naming server, by adding dependency of eureka-netflix-client in both the Microservices.

        7.  #LoadBalancing
		       - Let us understand load balancing between multiple instance of currency-exchange from currency-conversion.
		       - how to load balance, just by changing the @FeignClient url in proxy.
￼

![alt text](Currency Conversion Microservice.png)
        
## SpringCloud Gateway

            - Simple, yet effective way to route APIs.
            - Provide cross cutting concerns: like Security and monitoring/ metrics
- ![alt text](Spring Cloud Gateway.png)￼

        -  Add eureka discovery cleint  and reactive gateway dependency.  
        - Register the api-gateway with eureka-naming server by adding url in application-properties file
        - Exploring routes with Gateway, creating ApiConfiguration file.             
        - 

 ## Logging Filter
 ## Circuit breaker

![alt text](Circuit Breaker.png)

￼
	        
    -   we start playing with currency-exchange-service for circuit breaker, add dependencies.
	-   create circuit controller, play with @Retry.
	-   play with circuit breaker, rate Limiter, Bulkhead.