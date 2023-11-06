# keycloak-compose

Run keycloak and postgresql with docker compose

This repo is set for my blogs

[Run Keycloak in docker with extenal DB](https://medium.com/@ozbillwang/run-keycloak-in-docker-with-extenal-db-1b504ad00eae)

[Run Keycloak locally with Docker compose](https://medium.com/@ozbillwang/run-keycloak-locally-with-docker-compose-db9a9f2fb437)

### Quick start

Prerequisite
* [install docker](https://docs.docker.com/engine/install/)

Step 1

update `/etc/hosts` , add below lines

```
# keycloak
127.0.0.1 keycloak.com.au
```

On Windows, the file path is usually: `c:\Windows\System32\Drivers\etc\hosts`

Step 2
```
git clone https://github.com/ozbillwang/keycloak-compose.git
cd keycloak-compose
docker compose up -d
```
Step 3

access http://keycloak.com.au:8180
go with "Administration Console ",  then login with `admin / password `

![image](https://github.com/ozbillwang/keycloak-compose/assets/8954908/0c213c2a-cd7d-4235-b68a-43ca3dc809ec)
