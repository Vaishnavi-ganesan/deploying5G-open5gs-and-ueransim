## Deploying5G-open5gs-and-ueransim
Clone repository and build base docker image of open5gs, kamailio, ueransim
## Build docker images for open5gs EPC/5GC components
```
git clone https://github.com/herlesupreeth/docker_open5gs
cd docker_open5gs/base
docker build --no-cache --force-rm -t docker_open5gs .
```
## Build docker images for kamailio IMS components
```
cd ../ims_base
docker build --no-cache --force-rm -t docker_kamailio .
```
## Build docker images for UERANSIM (gNB + UE)
```
cd ../ueransim
docker build --no-cache --force-rm -t docker_ueransim .
```
## Build docker images for additional components
```
cd ..
set -a
source .env
sudo ufw disable
sudo sysctl -w net.ipv4.ip_forward=1
sudo cpupower frequency-set -g performance
```

# For 5G deployment only
'''
docker compose -f sa-vonr-deploy.yaml build
'''
## 5G SA deployment
## 5G Core Network
```
docker compose -f sa-vonr-deploy.yaml up
```
Open (http://<DOCKER_HOST_IP>:3000) in a web browser, where <DOCKER_HOST_IP> is the IP of the machine/VM running the open5gs containers. Login with following credentials
```
Username : admin
Password : 1423
```
## UERANSIM gNB 
```
docker compose -f nr-gnb.yaml up -d && docker container attach nr_gnb
```
## UERANSIM NR-UE 
```
docker compose -f nr-ue.yaml up -d && docker container attach nr_ue
```
