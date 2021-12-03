Installing Wireguard with Docker

Setting up VM
First I created a Digital ocean account and then installed these packages.
#sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
#curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

Then i got the docker source for my system which was 
#sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu$(lsb_release -cs)stable" && apt-cache policy docker-ce
and then changed into that repository. Then I installed docker. #sudo apt install docker-ce -y
Then I installed the docker-compose #sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
and then enabled it with #sudo chmod +x /usr/local/bin/docker-compose


Installing Wireguard

I first created the directorys for wireguard and wireguard/config and then created a docker-compose.yml file with these contents:

version: '3.8'

services:

wireguard:

container_name: wireguard

image: linuxserver/wireguard

environment:

- PUID=1000

- PGID=1000

- TZ=America/Chicago

- SERVERURL=137.184.97.98

- SERVERPORT=51820

- PEERS=pc1,pc2,phone1

- PEERDNS=auto

- INTERNAL_SUBNET=10.0.0.0

ports:

- 51820:51820/udp

volumes:

- type: bind

source: ./config/

target: /config/

- type: bind

source: /lib/modules

target: /lib/modules

restart: always

cap_add:

- NET_ADMIN

- SYS_MODULE

sysctls:

- net.ipv4.conf.all.src_valid_mark=1

     
I then ran  #docker-compose up -d  to start wireguard


I used #docker-compose logs -f wireguard  to get the qr code and added the vpn to my phone



     
     

     
     
     
     
     



      
