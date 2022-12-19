## first Microservice architecture 

### services/servers

1. config-server                 
2. gateway-server                
3. eureka_server_discovery       
4. first_microservice            
5. second_microservice           
6. keycloak                      
7. dev_vault
8. first_microservice_database   
9. elasticsearch                 
10. kibana                        
11. logstash                      
12. zipkin                        
13. kafka-server                  
14. zookeeper
15. redis-server                  

### How to Run

1. clone projects
    ```shell  
        git clone https://github.com/zakaria-shahen/firstMicroservicearchitecture
        cd firstMicroservicearchitecture
        git clone https://github.com/zakaria-shahen/firstmicroservice
        git clone https://github.com/zakaria-shahen/firstspringconfigurationserver
        git clone https://github.com/zakaria-shahen/firstSpringeurekaserverDiscovery
        git clone https://github.com/zakaria-shahen/fristSpringCloudGateway
        git clone https://github.com/zakaria-shahen/secondMicroservice
    ```
2. build images
    ```shell
        cd firstmicroservice 
        ./mvnw clean spring-boot:build-image -DskipTests
        cd ../firstspringconfigurationserver
        ./mvnw clean spring-boot:build-image -DskipTests
        cd ../firstSpringEurekaServerDiscovery
        ./mvnw clean spring-boot:build-image -DskipTests
        cd ../FristSpringCloudGateway
        ./mvnw clean spring-boot:build-image -DskipTests
        cd ../SecondMicroservice
        ./mvnw clean spring-boot:build-image -DskipTests
    ```
3. Run docker-compose
    ```shell
      docker-compose up
    ```

4. add keycloak to host 
   - windows
     - path: `C:\Windows\System32\drivers\etc`
     - add `127.0.0.1 keycloak` to host file

5. Vault Config

    [//]: # (TODO: move to docker-compose - run script after runing vault) 

    - run in vault container terminal
      ```shell
        vault login token=myroot
        vault kv put secret/firstmicroservice  test.vault="Hello, Vault"
      ```
    
#### services links

- [keycloak:8085](keycloak:8085)
- first_microservice [localhost:8080](localhost:8080)
- second_microservice [localhost:8081](localhost:8081)
- zipkin [localhost:9411](localhost:9411)
- elasticsearch [localhost:9200](localhost:9200)

### **NOTE**: not use IDEA to run config server with vault [ref](https://github.com/spring-cloud/spring-cloud-config/issues/1973#issuecomment-1312764350) but use cli with spring-boot:run
```shell 
./mvnw spring-boot:run
```

### redeploy one service

#### terminal

    ```shell
     ./mvnw install spring-boot:repackage -DskipTests &&
     docker-compose up --build --no-deps --force-recreate  -Vd  --no-log-prefix <service_name> 
    ```

#### IDEA

Add New Run Configuration:
- `install spring-boot:repackage  -DskipTests`
  ![docker_compose_with_idea](/README_IMAGE/docker_compose_with_idea.png)


### NOTE: if run with Eureka client config in your localhost (pc) without docker and your host (pc) installed docker-desktop.. problem may occur.. [link](https://stackoverflow.com/a/63283687/15107127)