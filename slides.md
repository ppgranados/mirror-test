# Consuming GCP PubSub messages with Spring batch

---

## Jose Granados

### Software Developer Architect
<!-- .slide: style="text-align: left;"> -->
<i class="fab fa-twitter"></i><a href="https://twitter.com/ppgranados">  @ppgranados</a><br>
<i class="fas fa-envelope"></i>  pp.granados@gmail.com<br>
<i class="fab fa-github"></i><a href="https://github.com/ppgranados">  github.com/ppgranados</a>

---

## Agenda
<!-- .slide: style="text-align: left;"> -->
- GPC Pub/Sub<br>
- Spring Batch<br>
- Use case<br>
- Demo<br>

---

# GPC Pub/Sub
<!-- .slide: style="text-align: left;"> -->
Cloud Pub/Sub is a fully managed real-time messaging service that allows you to send and receive messages between independent applications.

"Pub/Sub allows services to communicate asynchronously, with latencies on the order of 100 milliseconds"<br>

---

# GPC Pub/Sub
<!-- .slide: style="text-align: left;"> -->
<p align="center">
<img src="images/pubsub_diagram.png" width="549" height="522"/>
</p>

---

## Spring Batch
<!-- .slide: style="text-align: left;"> -->
Framework to create batch processing applications, provides reusable functions that are essential in processing large volumes of records, including logging/tracing, transaction management, job processing statistics, job restart, skip, and resource management.

Batch processing is used to process billions of transactions every day for enterprises.

Features:
- Transaction management
- Chunk based processing
- Declarative I/O
- Start/Stop/Restart
- Retry/Skip
- Web based administration interface

---

## Spring Batch key concepts
<!-- .slide: style="text-align: left;"> -->

- Jobs
- Steps
- Readers
- Writers
- Listeners

---

## Spring Batch key concepts
<!-- .slide: style="text-align: left;"> -->
<p align="center">
<img src="images/spring-batch-reference-model.png" />
</p>

---

## Namespaces
<!-- .slide: style="text-align: left;"> -->
Control what a container can see<br>
<br>
Used to control:-<br>
- Hostname within the container
- Processes that the container can see
- Mapping users in the container to users on the host

---

## File system
<!-- .slide: style="text-align: left;"> -->
- Containers cannot see the entire host's filesystem<br>
- They can only see a subset of that filesystem<br>
- The container root directory is changed

---

## Default networks
<!-- .slide: style="text-align: left;"> -->
<img src="images/docker_default_networks.png" style="float: right"/>

- bridge<br>
- host<br>
- none<br>

---

## Bridge network
<!-- .slide: style="text-align: left;"> -->
- Default network<br>
- Represents _docker0_ network<br>
- Containers communicate by IP address<br>
- Supports port mapping

---

## User defined networks
<!-- .slide: style="text-align: left;"> -->
- Docker provide multiple drivers<br>
- DNS resolution of container names to IP addresses<br>
- Can be connected to more than one network<br>
- Connect/disconnect from networks without restarting<br>

---

## Options for persisting data
<!-- .slide: style="text-align: left;"> -->
- Bind mounts<br>
- Data volume containers<br>
- Named volumes

---

## Building your own image
<!-- .slide: style="text-align: left;"> -->
- Custom images built from a file<br>
- Known as a dockerfile<br>
- Customise the image to grant permissions<br>
- Add databases to SQL Server<br>

---

## Dockerfile

<pre><code data-line-numbers="1|3|5-8|10|12|14">FROM mcr.microsoft.com/mssql/server:2019-CU5-ubuntu-18.04

USER root

RUN mkdir /var/opt/sqlserver
RUN mkdir /var/opt/sqlserver/sqldata
RUN mkdir /var/opt/sqlserver/sqllog
RUN mkdir /var/opt/sqlserver/sqlbackups

RUN chown -R mssql /var/opt/sqlserver

USER mssql

CMD /opt/mssql/bin/sqlservr
</pre></code>

---

## Docker container run

<pre><code data-line-numbers="1|2|3-8|9|10-13|14|15">docker run -d
--publish 15789:1433
--env SA_PASSWORD=Testing1122
--env ACCEPT_EULA=Y
--env MSSQL_AGENT_ENABLED=True
--env MSSQL_DATA_DIR=/var/opt/sqlserver/sqldata
--env MSSQL_LOG_DIR=/var/opt/sqlserver/sqllog
--env MSSQL_BACKUP_DIR=/var/opt/sqlserver/sqlbackups
--network sqlserver
--volume sqlsystem:/var/opt/mssql
--volume sqldata:/var/opt/sqlserver/sqldata
--volume sqllog:/var/opt/sqlserver/sqllog
--volume sqlbackup:/var/opt/sqlserver/sqlbackups
--name sqlcontainer1
mcr.microsoft.com/mssql/server:2019-CU5-ubuntu-18.04
</pre></code>

---

## What is Compose?
<!-- .slide: style="text-align: left;"> -->
"Compose is a tool for defining and running multi-container Docker applications.
With Compose, you use a YAML file to configure your application`s services.
Then, with a single command, you create and start all the services from your configuration."<br>
<font size="6"><a href="https://docs.docker.com/compose/">docs.docker.com/compose</a></font>

---

## Resources
<!-- .slide: style="text-align: left;"> -->
<font size="6">
<a href="https://github.com/dbafromthecold/DockerDeepDive">https://github.com/dbafromthecold/DockerDeepDive</a><br>
<a href="http://tinyurl.com/y3x29t3j/summary-of-my-container-series">http://tinyurl.com/y3x29t3j/summary-of-my-container-series</a><br>
<a href="https://github.com/dbafromthecold/SqlServerAndContainersGuide">https://github.com/dbafromthecold/SqlServerAndContainersGuide</a>
</font>

<p align="center">
<img src="images/dockerdeepdive_qr_code.png" />
</p>
