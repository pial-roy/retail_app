1. Use appropriate docker-compose for your machine. Dockerfile and docker-entrypoint.sh only needed for raspberry-pi.
Because official image is not available at docker hub.

Only change docker-compose.yml as per your machine.

a. docker compose for regular Mac/Windows/Linux/Or any x64/x86 architecture machine) :

version: '3.8'

services:
  mongodb:
    image: mongo:latest
    container_name: mongodb_container
    environment:
      MONGO_INITDB_ROOT_USERNAME: db_admin
      MONGO_INITDB_ROOT_PASSWORD: db_admin
      MONGO_INITDB_DATABASE: db_ware
    ports:
      - "27017:27017"
    volumes:
      - ./data/db:/data/db
    restart: unless-stopped


b. docker compose for raspberry :

First load the unofficial image example :
docker load --input mongodb.ce.pi4.r6.0.10-mongodb-raspberrypi-docker-unofficial.tar 

Then use this docker-compose

version: '3.8'

services:
  mongodb:
    build:
      context: .
      dockerfile: Dockerfile  # Name your Dockerfile as 'Dockerfile' in the same directory
    container_name: mongodb_container
    environment:
      MONGO_INITDB_ROOT_USERNAME: db_admin
      MONGO_INITDB_ROOT_PASSWORD: db_admin
      MONGO_INITDB_DATABASE: db_ware
    ports:
      - "27017:27017"
    volumes:
      - ./data/db:/data/db
      - ./data/configdb:/data/configdb
    restart: unless-stopped
			
	