# Microservices
https://www.linkedin.com/my-items/saved-posts/
Microservices Notes:>> https://eazybytes.com/#courses
Course:> 'Master Microservices with SpringBoot,Docker,Kubernetes' 
Its a course of 39.5 hours.. I have spend 120.5 hours to complete the course for hands-on knowledge..

1: http://localhost:8080/h2-console/ :> H2 console/
2: http://localhost:8080/swagger-ui/index.html :> Swagger UI..
   in order to get Swagger UI documentation for the API, add dependency injection in Accounts pom.xml
   <dependency>
			<groupId>org.springdoc</groupId>
			<artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
			<version>2.6.0</version>
   </dependency>
   
Uname for Docker: pgourish87
docker version
There are 3 approaches to generate docker image:
1: dockerfile, 2: BuildPacks, 3: GoogleJib
1: dockerfile

to create a docker image: docker build . -t pgourish87/accounts:s4
to check docker image: docker images
TO run docker container from docker image: 
PS C:\Drive_D\Microservices\section4\accounts> docker run -d -p 8080:8080 pgourish87/accounts:s4
Then container Id will be generate: ce5a0b9bf66e2eeaf38a6ba04b9deaf15681b36ff3ea6e77ce3cca69bbbabff3
List of running cmds: docker ps

2: BuildPacks
PS C:\Drive_D\Microservices\section4\loans> mvn spring-boot:build-image
There is issue with BuildPack

3: GoogleJib:
PS C:\Drive_D\Microservices\section4\loans> mvn compile jib:dockerBuild
add in pom.xml
<packaging>jar</packaging>
<build>
		<plugins>
		 <plugin>
			<groupId>com.google.cloud.tools</groupId>
			<artifactId>jib-maven-plugin</artifactId>
			<version>3.4.1</version>
			<configuration>
				<to>
					<image>pgourish87/${project.artifactId}:s4</image>
				</to>
			</configuration>
		 </plugin>
	    </plugins>
	</build>
	
PS C:\Drive_D\Microservices\section4\cards> docker images
TO run docker container from docker image: 
 docker run -d -p 9000:9000 pgourish87/cards:s4
 docker run -d -p 8090:8090 pgourish87/loans:s4
 
Pushing docker images from local to remote docker hub repository
PS C:\Drive_D\Microservices\section4\cards> docker image push docker.io/pgourish87/accounts:s4

Pull docker images from docker hub to local repository
PS C:\Drive_D\Microservices\section4\cards> docker pull pgourish87/cards:s4

In Order to create docker image in local repository: 
PS C:\Drive_D\Microservices\section4\cards> docker pull pgourish87/accounts:s4

Then run a docker image as container: 
PS C:\Drive_D\Microservices\section4\cards> docker run -d -p 8080:8080 pgourish87/accounts:s4

We can define all our microservices in docker-compose.yml file.
Run a cmd from the parent folder where docker-compose.yml file is present
PS C:\Drive_D\Microservices\section4\accounts> docker compose up -d

In order to down all the container: docker compose down

Activating Spring Profile using 3 approches:
1: cmd line arguments in AccountsApplication main class in edit configurations
--spring.profiles.active=prod --build.version=1.1
2: JVM System Varialble
 VM options: -Dspring.profiles.active=prod -Dbuild.version=1.3
3: Environment Variables
 Environment variable: SPRING_PROFILES_ACTIVE=prod:BUILD_VERSION=1.8;
 
1: first preference will be always excecute cmd line arguments.
2: JVM system variables is the next preferance
3: Environemnt variables will be the 3 preferance.

# There are 3 approaches to get configuaration in microservic application
1: classpath: in resource folder, place all configuaration.yml files & define it in respective service(accounts) .yml file
2: FileSystem: place all configuaration files in local system, n define it in config server
3: Github repository: define all configuaration in Github repository n mention the path in config server .yml file

# latest RabbitMQ 3.13
docker run -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.13-management

*In order to push docker images to docker hub repository:
C:\Drive_D\Microservices\section7>docker image push docker.io/pgourish87/accounts:s6
C:\Drive_D\Microservices\section7>docker image push docker.io/pgourish87/loans:s6
C:\Drive_D\Microservices\section7>docker image push docker.io/pgourish87/cards:s6
C:\Drive_D\Microservices\section7>docker image push docker.io/pgourish87/configserver:s6

* pushed accounts, cards, loans, & configserver services to docker hub with s6 tag.
* created docker compose folder for default, Prod, qa:> inside that placed docker-compose.yml file.
* in order to create the images for the respective services & create docker container, please pull the docker images form docker hub
* then place the docker-compose.yml file in a local: go to the folder where docker file has been created
* Ex: C:\Drive_D\Microservices\section6\v2-spring-cloud-config\docker-compose\qa> docker compose up -d
* Then test it in postman. 

* To create mysql db using docker,
C:\Drive_D\Microservices\section7>docker run -p 3306:3306 --name accountsdb -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=accountsdb -d mysql
C:\Drive_D\Microservices\section7>docker run -p 3307:3306 --name loansdb -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=loansdb -d mysql
C:\Drive_D\Microservices\section7>docker run -p 3308:3306 --name cardsdb -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABASE=cardsdb -d mysql

Section 7:
To create the docker image for accounts, loans, cards, configserver, 
C:\Drive_D\Microservices\section7\accounts>mvn compile jib:dockerBuild
C:\Drive_D\Microservices\section7\loans>mvn compile jib:dockerBuild
C:\Drive_D\Microservices\section7\cards>mvn compile jib:dockerBuild
C:\Drive_D\Microservices\section7\configserver>mvn compile jib:dockerBuild

Note: created docker-compose yaml files for default, Prod, QA environemnt. directly we can establish database connection from docker-compose.yaml file. no need to create manually.
pushed all the docker images related to section 7 to docker hub
1:docker image push docker.io/pgourish87/configserver:s7
2:docker image push docker.io/pgourish87/accounts:s7
3:docker image push docker.io/pgourish87/loans:s7
4:docker image push docker.io/pgourish87/cards:s7

How to create docker image & docker container from Docker hub
1: try to pull docker image to local by entering the cmd: docker pull pgourish87/configserver:s6, docker pull pgourish87/accounts:s6, docker pull pgourish87/loans:s6, docker pull pgourish87/cards:s6
2: then copy the docker compose file into ur local.(default, prod, qa)
3: then enter the cmd: docker compose up -d.
4: then all the containers are going to start.. we can test the services in postman.

To delete all the containers: go to path:
C:\Drive_D\Microservices\section7\docker-compose\default>docker compose down

Eureka Dashboard to registered all services: http://localhost:8070/
URL of Eureka server: http://localhost:8070/eureka/apps

Note: In Postman, gatewayserver project:> please use the below API URL
1: http://localhost:8072/ACCOUNTS/api/create for post
2: http://localhost:8072/ACCOUNTS/api/fetch?mobileNumber=4354437687 for get

Note: Always try to run ConfigServer app, EurekaServer App, then Accounts, Loans, Cards, at last run gatewayserver App..

*To create a redis container inside docker:
docker run -p 6379:6379 --name eazyredis -d redis

section_11/docker-compose/observability/alloy/alloy-local-config.yaml
section_11/docker-compose/observability/grafana/datasource.yml

Grafana is going to start at port: localhost:3000
Prometheus Dashboard running on port : localhost:9090/targets
Grafana dashboard: http://localhost:3000/login
Uname: admin
Pwd: admin(Vihaan@1509)

From a terminal, enter the following command to start Keycloak:
docker run -d -p 7080:8080 -e KEYCLOAK_ADMIN=admin -e KEYCLOAK_ADMIN_PASSWORD=admin quay.io/keycloak/keycloak:25.0.2 start-dev

Keycloak dashboard URL: localhost:7080
uname: admin & pwd: admin
in order to up Keycloak, we need to start docker container with port 7080:8080

Note:Microservices security related check at postman.
I have written docker compose file in section 12, please navigate to 
PS C:\Drive_D\Microservices\section_12\docker-compose\prod> docker compose up -d
before that make sure to stop running conatiner of Keycloak: which is running on port 7080:8080
then watch video 197 from the course in order to test all the services via postman.

Installing RabbitMQ in local: make sure to run docker desktop inside a system
# latest RabbitMQ 3.13
docker run -d -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.13-management

RabbitMQ console: localhost:15672
Uname: guest & pwd: guest

Setup apache kafka inside local system.
link: https://kafka.apache.org/
https://kafka.apache.org/quickstart
Kafka with KRaft
Note: Instead of installing apache kafka server in local, better to create image of it in docker dekstop. by giving instrauctionsin docker-compose.yml file. 
https://bitnami.com/
Bitnami makes it easy to get your favorite open source software up and running on any platform, including your laptop, Kubernetes and all the major clouds. 
In addition to popular community offerings, Bitnami, now part of VMware, provides IT organizations with an enterprise offering that is secure, compliant, 
continuously maintained and customizable to your organizational policies.
It is used to run apache kafka inside docker container, instaed of making local setup
Use the docker compose instrauctions provided by bitnami.(update docker-compose.yml file inside section 13)
* Steps to run Event driven Microservices using kafka, Spring cloud functions & stream.(Section-14 of 217 video)
1: Write the instrauctions in docker-compose.yml file by using tag s14
2: Set the Keycloak client administration for creating client, Realm Roles & set the roles for the client.. 
3: run the cmd: docker compose up -d
4: after running all the containers which are specified, the check the services in postman.
5: http://localhost:8072/eazybank/accounts/api/create
6: make sure to do authorization in postman, by adding the client id & client secret

Note: All the code related to section 14 is there in section 13 code. related to RabbitMQ all changes are in section13, s13 tag, related to kafka all changes are in section13 s14 tag.


kubectl config get-contexts
kubectl config use-context docker-desktop
kubectl get nodes


Kubernetes dashboard bearar token:
eyJhbGciOiJSUzI1NiIsImtpZCI6ImJrX1U3MnY1WnQybTlGMFVnOGk5R3RZWVkyNVMySkJRYmtCYzREZHNYd28ifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJhZG1pbi11c2VyIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImFkbWluLXVzZXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiJlZjRmYWRhNS0wOGQ4LTQ0NmUtODY0OC1mMTE5MWJlM2IxZTYiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZXJuZXRlcy1kYXNoYm9hcmQ6YWRtaW4tdXNlciJ9.R3nxZvUpjSEeuLfb4mOF_F6GY2yBaJWBFavvPm_oMEauHc1tg2smsLnjOsSgVkSaeYJovHGTLp9jzNshpTJF-kMAqWlXKCIMfppmrj45rCAQJx0013wWj660eughM3aV5l4WRFvHutm66kutQJZN5tY9h1guSdxxMpB_IoC1LeQkSj7urffw6fx6VXe_yvRT6vF_sQ-TMSQYcTJV_-OsehLzIIXZK1m10gBqeqsIu6Zc39WwTYLVFYUOoF7Hd-TBgJRCbNOTT3rQO6YXfmU3IdJbKZFMU1MQO6USraWmDxqa_0S_wjyMxumWPdd4hMiJSRAGRWmwR-cnr8QTUX7dMA

To access Dashboard run:
  kubectl -n kubernetes-dashboard port-forward svc/kubernetes-dashboard-kong-proxy 8443:443
  
Kubernetes dashboar: localhost:8443

in order to create .yml file inside a folder we can use below cmd.
PS C:\Drive_D\Microservices\section_15\kubernetes> notepad configserver.yml & touch configserver.yml in linux
kubectl get deployments

Deploying all microservices to Kubernetes cluster:>>>
PS C:\Drive_D\Microservices\section_15\kubernetes> kubectl apply -f 1_keycloak.yml
PS C:\Drive_D\Microservices\section_15\kubernetes> kubectl apply -f 2_configmaps.yml
PS C:\Drive_D\Microservices\section_15\kubernetes> kubectl apply -f 3_configserver.yml

for deleting all Microservices from Kubernetes cluster
PS C:\Drive_D\Microservices\section_15\kubernetes> kubectl delete -f 8_gateway.yml

Note: Add the bitnami repository to helm chart installation steps for Kubernetes clusters:
helm repo add bitnami https://charts.bitnami.com/bitnami


To create a helm chart inside a local:
PS C:\Drive_D\Microservices\section_16\helm> helm create eazybank-common

To compile the helm chart to a particular microservices, 
PS C:\Drive_D\Microservices\section_16\helm\eazybank-services\accounts> helm dependencies build

Keycloak can be accessed through the following DNS name from within your cluster:

    keycloack-keycloak.default.svc.cluster.local (port 80)

Keyclock admin dashboard: Localhost:80

Uname: user
password: password

PS C:\Drive_D\Microservices\section_16\helm> echo "Browse to http://127.0.0.1:8080" kubectl port-forward svc/grafana 3000:3000

echo "Browse to http://127.0.0.1:8080" kubectl port-forward svc/grafana 8080:3000 "&"

To start garfana server:
PS C:\Drive_D\Microservices\section_16\helm\environments> kubectl port-forward svc/grafana 3000:3000
Get the admin credentials:>>> for accessing grafana in local:localhost:3000
"User: admin"
"Password:"lKKksVECgr"

in order to delete all services from Kubernetes cluster use the below cmd
PS C:\Drive_D\Microservices\section_16\helm>helm uninstall eazybank
PS C:\Drive_D\Microservices\section_16\helm> helm ls

Section 12: contains authorization Oauth 2.0 changes.. in order to check the API, we can use docker compose file for the tag s12 from docker hub repo
PS C:\Drive_D\Microservices\section_12\docker-compose\prod> docker compose up -d
Keyclock: localhost:7080
username: admin, Password: admin
Create client eazybank-callcenter-cc & Assign Realm roles as well..
Postman API is: gatewayserver_security in postman.
Then remove all running conatiners from docker..
PS C:\Drive_D\Microservices\section_12\docker-compose\prod> docker compose down


<<<Microservices Notes from 27-09-24>>>
1:>Starter Project used in Spring Boot Microservices:
	Spring Web, H2 Database, Spring Data JPA, Spring Boot Actautor, Spring Boot Devtools, Lombok, Validation, & so on
2:> Used Codium plugin inside IntelliJ Idea to get Support for Coding.
3:> 
