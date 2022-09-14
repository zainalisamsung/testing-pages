## Demo for deploying all services of the Healthcare Research Platform by using docker.

Firstly, creates a docker network hrp to connect docker containers by using the following syntax:


**1.    Creates Network**

Firstly, creates a docker network **hrp** to connect docker containers by using the following syntax:

` docker network create hrp `

**2.    Deploy Postgres**

After creating the network, deploy Postgres by following below instructions.

Pull(Download) the latest or specific version of Postgres images by:

`docker pull postgres14.5`

Now run the Postgres container, the command will start the Postgres container under hrp-Postgres and expose the environmental password of the variable name POSTGRES_PASSWORD with value docker to the container. Further launches the container in the background and Bind port 5432 on localhost to port 5432 within the container.



`Docker run --name hrp-postgres –network`

Now, clone the latest version with:

`git clone: https://github.com/HealthcareResearchHub/research-platform.git - Connect to preview`

The git clone command will download the repository that already exists on GitHub including all files.

**3.    Create File “Create-account-key.json file**



Now create a json file in the research platform directory. For that change directory by using below command:

`cd research-platform`

Further, list all files and directories of platform by using ls command.

ls platform/service-account-key.json

**    4.    Deploy platform**

Now deploy the platform by following below instructions.

The first step is to build by using grade build command that creates a jar file of the application and test the code formatting by:

`./gradlew : platform: build -x detekt -x test -x ktlintMainSourceSetCheck

After successful gradle build, build docker image of hrp-platform 0.1.0 in platform` directory by using command:

`docker build --tag hrp-platform: 0.1.0 ./ platform/`

Now list all images and containers within hrp network by Grep command.

`docker images | grep hrp`

After a successful build, run the container of hrp-platform.

`docker run --name hrp -platform- network hrp.`

To ensure hrp-platform container is running use Ps command.

`docker ps`

Now run the Postgres Container under hrp-Postgres and expose the environmental password of the variable name POSTGRES_PASSWORD with value docker to the container. Further launches the container in the background and Bind port 5432 on localhost to port 5432 within the container

`docker run --name hrp -platform- network hrp-platform -- network hrp -e DB_HOST=471C510ee91b -e DB_PORT= 5432 -e DB_NAME = postgres`

To ensure Postgres container is running use Ps command

`Docker ps`

Now connect to local host:3030/api/projects.

`curl -i local host:3030/api/projects`

**    5.    Deploy trino**

Now, deploy trino by creating new directory trino/etc/catlog and then change directory:

`mkdir trino`

`cd trino`

`mkdir etc`

`cd etc`

`mkdir catlog`

Now, save the jvm.config file

`echo"\

-server

-Xmx16G

-XX: InitialRAMPercentage=80

-XX:MaxRAMPercentage=80

-XX:G1HeapRegionSize=32M

-XX:+ExplicitGCInvokesConcurrent

-XX:+ExitOnOutOfMemoryError

-XX:+HeapDumpOnOutOfMemoryError

-XX:-OmitStackTraceInFastThrow

-XX : ReservedCodeCacheSize=512M

-XX:PerMethodRecompilationCutoff=10000

-XX:PerBytecodeRecompilationCutoff=10000

-Djak.attach.allowAttachSelf=true

-Didk.nio.maxCachedBufferSize=2000000

-XX:+UnlockDiagnosticVMOptions

-XX:+UseAESCTRIntrinsics\

">jvm.config`

After successful configuration pull(download) trinodb/trino 393 version.

`docker pull trinodb/trino:393`

Further run the hrp-trino container from trinodb/trino image and map hrp-trino default port 8080 to inside of container port of 8080 by using following command.

``docker run --name hrp-trino --network hr-d -p 8080:8080 --volume $HOME/research-platform/trino/``

**    6.    Deploy data-query-service**



Now, deploy data-query-service. First step is to change directory and build application data-query-service and generate a jar file, performing a code test.

``cd../..``

`/gradlew : data-query-service:build -x detekt -x test`

After gradle build and code test build docker image of data-query-service tag 0.1.0 in data-query-service directory.

`docker build -tag hrp-data-query-service:0.1.0./data-query-service`

Now list all images related to hrp-a-query-service by using grep command.

`docker images | grep hrp-data-query-service`

Now run hrp-data-query-service container, exposing the environmental USER with value docker to the container and skipping the first line

`docker run--name hrp-data-query-service--network-eCATALOG_USER=postgres-eTRINO_HOST=$(dockerps p-trino | awk ' (print $1}`

To ensure successful run, ps command will be used to show all running containers

`docker ps`

**    7.    Test API calls**

Now test API calls by using CURL command.

Curl POST command will initiate a POST Request to connect and further transfer data.

`curl -i -request POST localhost: 3030/api/projects`

`curl - localhost:3030/api/projects`

Now run the PSQL Queries within the postgres database and request to connect the application running inside of the container on 3031 port.

`psql -h localhost -p 5432 -U postgres -d postgres`

`curl-i -request POST localhost: 3030/api/projects --header 'Content-Type: application/json

--data-raw

"{

"name":

"'string"

"isOpen": true,

"info": {

'additionalProp1": (],

"additionalProp2": {],

"additionalProp3": (}

]`

To ensure containers are running use ps command.

`docker ps`

Further to retrieve logs of the container present at the time of execution use:

`docker logs 645807fb79c`

Now to stop and remove containers use following syntax. Further to ensure running container list use docker ps.

`docker stop 645807fb79c`

`docker rm 645807fb79c`

`docker ps -a`

Now run hrp-data-query-service

`docker run --name hrp-data-query-service --network hrp -e debug=true -e CATALOG_USER=postgres -e TRINO_

T=$ (docker ps | grep hrp-trino | awk

" {print $17°) -e TRINO_PORT=8080 -e TRINO_CATALOG=postgresql -d -p 3031:3030 hrp-data-query-service:0`

After Successful run, connect to localhost:3030/api/projects

`curl -i localhost:3030/api/projects`

Now to see whether Data Query service and platform works use following log command.

`docker logs 4eb10f4c17e9`
