# OpenMRS Platform

**Please note that this  is [OpenMRS platform](http://openmrs.org/download/), modules must be installed for a functional EMR system. Use [giorgioazzinnaro/openmrs-reference-application](https://hub.docker.com/r/giorgioazzinnaro/openmrs-reference-application) if you would like to get the default distribution.**

## Configuration

This runs on top of [tomcat:8-jre8-alpine](https://hub.docker.com/_/tomcat/).

OpenMRS depends on [mysql:5.6](https://hub.docker.com/_/mysql/) as database.

```
docker run --name openmrs-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:5.6
```

Now run one `giorgioazzinnaro/openmrs-platform` instance linking it to `openmrs-mysql` (keep your MySQL password in mind for first configuration) and expose port 8080.

```
docker run --name openmrs-platform --link openmrs-mysql:mysql -it -p 8080:8080 giorgioazzinnaro/openmrs-platform:2.0.5
```

Point your browser to [localhost:8080/openmrs](http://localhost:8080/openmrs), and select `Advanced` installation.  
Change your database connection string to look like this:

```
jdbc:mysql://mysql:3306/@DBNAME@?autoReconnect=true&sessionVariables=default_storage_engine=InnoDB&useUnicode=true&characterEncoding=UTF-8
```
*note `localhost` was replaced with `mysql`*

Go on selecting the database to be created:

*Do you currently have an OpenMRS database installed that you would like to connect to?*: `No`  
*username*: root  
*password*: `$MYSQL_ROOT_PASSWORD` (*the password you selected for your MySQL container*)

On the next page, again enter `$MYSQL_ROOT_PASSWORD` so that the installer creates the new user.

Now change all other settings as desired.

After configuration the homepage should note that only the REST API can be used at this stage, if at this point you expected a fully functional EMR system, you should move over to [giorgioazzinnaro/openmrs-reference-application](https://hub.docker.com/r/giorgioazzinnaro/openmrs-reference-application), the default OpenMRS implementation.

## Modules installation

Because this is just OpenMRS platform, modules should be added to provide functionality.

Modules are installed at `/root/.OpenMRS/modules`, which is exposed as volume, so either that can be mounted, or files can be copied over to the container.


## docker-compose

For ease of use, a `docker-compose.yml` file is provided in the GitHub repo.
