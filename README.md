# keycloak-compose

Run keycloak and postgresql with docker compose by two scenarios
* [Keycloak with simple instance](https://github.com/ozbillwang/keycloak-compose/blob/master/README.md#keycloak-single-instance-with-docker-compose-up-and-running-inseconds)
* [Keycloak with cluster (experimental for your understanding of how Keycloak clusters work)](https://github.com/ozbillwang/keycloak-compose/blob/master/README.md#keycloak-cluster-with-docker-compose-up-and-running-inseconds)

This repo is mainly set for my blogs

[Run Keycloak in docker with extenal DB](https://medium.com/@ozbillwang/run-keycloak-in-docker-with-extenal-db-1b504ad00eae)

[Run Keycloak locally with Docker compose](https://medium.com/@ozbillwang/run-keycloak-locally-with-docker-compose-db9a9f2fb437)

[Keycloak Cluster with Docker Compose — Up and Running in Seconds](https://medium.com/@ozbillwang/keycloak-cluster-with-docker-compose-up-and-running-in-seconds-0c3223df4f75)

[Keycloak Backup and Restore](https://medium.com/@ozbillwang/keycloak-backup-and-restore-eea87d294b3c)

## Keycloak (single instance) with Docker Compose - Up and Running in Seconds

### Prerequisite
* [install docker](https://docs.docker.com/engine/install/)

### Step 1

update `/etc/hosts` , add below lines

```
# keycloak
127.0.0.1 keycloak.com.au
```

On Windows, the file path is usually: `c:\Windows\System32\Drivers\etc\hosts`

### Step 2
```
git clone https://github.com/ozbillwang/keycloak-compose.git
cd keycloak-compose
docker compose up -d
docker ps -a
```
make sure all containers running well
### Step 3

access http://keycloak.com.au:8180
go with "Administration Console ",  then login with `admin / password `

![image](https://github.com/ozbillwang/keycloak-compose/assets/8954908/0c213c2a-cd7d-4235-b68a-43ca3dc809ec)

## Keycloak Cluster with Docker Compose - Up and Running in Seconds

Yes, the solution is ready now with help from Niko Köbler (@dasniko) with [his cool video](https://www.youtube.com/watch?v=P96VQkBBNxU)

### Step 1

update `/etc/hosts` , add below lines

```
# keycloak
127.0.0.1 keycloak.com.au
```

On Windows, the file path is usually: `c:\Windows\System32\Drivers\etc\hosts`

### Step 2
```
git clone https://github.com/ozbillwang/keycloak-compose.git
cd keycloak-compose
docker compose -f docker-compose-cluster.yml up -d
```

* Check the health
```
docker ps -a
```
![image](https://github.com/ozbillwang/keycloak-compose/assets/8954908/e97b6999-4464-4bad-94c6-39aaed386309)

* Check logs with Cluster events

```
docker logs -f <kc1 or kc2 container id>
```

![1_rSVRVOgXGqqzCHgmZmaAAg](https://github.com/ozbillwang/keycloak-compose/assets/8954908/d62be1fc-5468-41de-b964-eaee0079d4f4)

* Check the cluster logs, there should be two members in cluster pool now

>Received **new cluster view for channel** ISPN: [b31f28d4c94a-31765|1] (2) [b31f28d4c94a-31765, bc873530c08b-24274]
>Starting rebalance with members [b31f28d4c94a-31765, bc873530c08b-24274]


### Step 3

access http://keycloak.com.au:8180

go with "Administration Console ",  then login with `admin / password `

### step 4
Test the fail over and cluster realiable.

* kill one keycloak container
```
docker ps -a
docker rm -f keycloak-compose-kc2
```
Check logs, you will only see one member in Cluster pool now.

>  Updating cache members list [b31f28d4c94a-31765], topology id 6

![image](https://github.com/ozbillwang/keycloak-compose/assets/8954908/3bf3e0ea-e11a-4553-aba3-e61e9c288fb4)

When you refresh the website http://keycloak.com.au:8180, it takes about 5~10 seconds at first time, then work as normal

* restore all services
```
$ docker compose -f docker-compose-cluster.yml up -d

 ✔ Container db     Running 
 ✔ Container kc1    Started    # because I killed it before
 ✔ Container kc2    Running 
 ✔ Container kc_lb  Running
```
Check logs again, two members in cluster pool now. 

If you access http://keycloak.com.au:8180, it works fine

> Starting rebalance with members [b31f28d4c94a-31765, 462ae7fcf1a3-41736], phase READ_OLD_WRITE_ALL, topology id 7
> Finished rebalance with members [b31f28d4c94a-31765, 462ae7fcf1a3-41736], topology id 10

![image](https://github.com/ozbillwang/keycloak-compose/assets/8954908/728ee4a4-10a2-4b8e-a6d7-8641fde0f6c0)

### Reference

https://www.keycloak.org/2019/05/keycloak-cluster-setup.html

https://www.youtube.com/watch?v=P96VQkBBNxU

https://www.keycloak.org/2019/08/keycloak-jdbc-ping

http://jgroups.org/manual/#_jdbc_ping
