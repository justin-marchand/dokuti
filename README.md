[![CircleCI](https://circleci.com/gh/GrindrodBank/dokuti.svg?style=svg)](https://circleci.com/gh/GrindrodBank/dokuti)
[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2FGrindrodBank%2Fdokuti.svg?type=shield)](https://app.fossa.com/projects/git%2Bgithub.com%2FGrindrodBank%2Fdokuti?ref=badge_shield)

# Dokuti

Dokuti is a batteries-included, enterprise-grade, open-source, free-standing microservice document service suitable for use as a back-end for document storage and versioning needs.

This service is implemented in Java using [Spring Boot](https://spring.io/projects/spring-boot).

## Quickstart

Taking it out for a spin in 5-10 minutes:

* Clone Github repo:
```bash
git clone git@github.com:GrindrodBank/dokuti.git
```

* Run using docker-compose:
```bash
cd dokuti
cd quickstart/docker-compose
docker-compose up
```

3 services are installed:
* Web 1 - The actual document service installed on port 8181
* PostgreSQL Database installed on port 5432
* Keycloak Authentication service installed on port 9191 and can be accessed by going to http://localhost:9191

### Credentials for PostgreSQL database
* Password: postgres
* Database: dokuti

### Credentials for Keycloak
* Username: admin
* Password: admin

> This example uses:
> * [curl](https://github.com/curl/curl) (a handy command line client to do HTTP requests) and 
> * [jq](https://stedolan.github.io/jq/) ( a nice command line JSON processor)

* Get an authentication token in order to get API access:

```bash
TOKEN=`curl \
-s -v \
-X POST http://localhost:9191/auth/realms/Dokuti/protocol/openid-connect/token \
-H 'Content-Type: application/x-www-form-urlencoded' \
-H 'Bearer-Token: 575c92bb-33fe-45b8-85ef-7cc5710e62eb' \
-H 'cache-control: no-cache' \
-d 'grant_type=password&username=test-user&password=test-user&client_id=dokuti&client_secret=ee5c1c57-bf2f-43e6-9025-49344113c88d' \
| jq '.access_token' -r` \
&& echo "TOKEN is :$TOKEN"
```

* Create a `sample.file.txt` file to upload as a document:

```bash
echo "Hello there" >> sample.file.txt
```

* Then create a document:
```bash
curl -s -v  \
-H "Accept: application/json"  \
-X POST http://localhost:8181/documents \
-H "Authorization: Bearer $TOKEN" \
-H "cache-control: no-cache" \
-H "Content-Type: multipart/form-data" \
-H "Transfer-Encoding: chunked" \
-F "description=test initial description." \
-F file=@sample.file.txt | jq
```

* To undeploy everything:

```bash
docker-compose down
```

# Postman Collection

A `Dokuti.postman_collection.json` Postman collection has been included within the `postman` folder. This collection is intended for exploring the Dokuti API. There is also a `Dokuti-Quickstart.postman_environment.json` Postman environment that can be used to interact with the
Dokuti instance deployed using the `docker-compose` method in the Quickstart above.

# Project Documentation

All project documentation is currently available within the `/doc` folder.

## License
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2FGrindrodBank%2Fdokuti.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2FGrindrodBank%2Fdokuti?ref=badge_large)