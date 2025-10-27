### 1-1 For which reason is it better to run the container with a flag -e to give the environment variables rather than put them directly in the Dockerfile?

Because -e keeps passwords and configs out of the image, making it more secure.  
With ENV in the Dockerfile, the data is stored and visible in the image.

### 1-2 Why do we need a volume to be attached to our postgres container?

Because without a volume, all the data is lost when the container is removed.  
The volume keeps the data on the host machine, so we can recover it later if we want.

### 1-3 Database container essentials

#### Dockerfile
```Dockerfile
FROM postgres:17.2-alpine
COPY CreateScheme.sql /docker-entrypoint-initdb.d/
COPY InsertData.sql /docker-entrypoint-initdb.d/

# 2. Build the image
docker build -t stiivelee/postgres-init .

# 3. Run the container
docker run -d --name db-init \
  --network app-network \
  -e POSTGRES_USER=usr \
  -e POSTGRES_PASSWORD=pwd \
  -e POSTGRES_DB=db \
  -v "$(pwd)/data":/var/lib/postgresql/data \
  stiivelee/postgres-init
```
### 1-4 Why do we need a multistage build?

We use a multistage build to make the final image lighter and faster.  
It separates the part where we build the app from the part where we run it.

#### Dockerfile
```dockerfile
#Use a Java JDK
FROM eclipse-temurin:21-jdk-alpine AS myapp-build

#Define a working directory for the app inside the container.
ENV MYAPP_HOME=/opt/myapp
WORKDIR $MYAPP_HOME

#Install Maven to build the project
RUN apk add --no-cache maven

#Copy the project files inside the container
COPY pom.xml .
COPY src ./src
RUN mvn package -DskipTests

#Start a new lightweight image with only the JRE
FROM eclipse-temurin:21-jre-alpine

#Copy the compiled JAR into the final image
ENV MYAPP_HOME=/opt/myapp
WORKDIR $MYAPP_HOME
COPY --from=myapp-build $MYAPP_HOME/target/*.jar $MYAPP_HOME/myapp.jar

#Define the command to run the app when the container starts
ENTRYPOINT ["java", "-jar", "myapp.jar"]
```

### 1-5 Why do we need a multistage build?
A reverse proxy acts as a middle man between clients and servers, improving security,

### 1-6 Why is Docker Compose so important?
Docker Compose is important because it simplifies the management of multiple containers.

### 1-7 Why is Docker Compose so important?

The main commands are docker compose up -d --build to start and build containers, docker compose down to stop and remove them, docker compose ps shows running containers, and docker compose logs -f displays their logs.

### 1-8 Document your Docker Compose file.

My Docker Compose file defines three services: a PostgreSQL database, a Java backend, and an Apache HTTP proxy.
The proxy redirects external traffic to the backend, which connects to the database internally.

### 1-9 Document your publication commands and published images in dockerhub.

I used the commands docker tag postgres:17.2-alpine stiivelee/postgres-db:1.0 and docker push stiivelee/postgres-db:1.0 to publish my image.

### 1-10 Why do we put our images into an online repo?

We put our images into an online repository like Docker Hub to share them easily, reuse them on other machines, and simplify deployment without rebuilding the image each time.


