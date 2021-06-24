# Run as Docker containers
Both Execution Core and Data App are dockerized and can be run as docker container.
Since in the tipical deployment scenario there will be 4 instances (2 Data App + 2 ECC), 2 for sender (consumer) and 2 for receiver (provider) side, a docker-compose file is provided.

## Run with docker-compose and public images
Each service defined in the docker-compose will use images published on Platoon Docker Hub [here]().
Once you have correctly configured the provided configuration files (see section above #docker-configuration) you can run:

```docker-compose up```


### (Alternative) Build Docker images from sources
Alternatively you can build images both for ECC and Data App directly from sources, located in the following repositories:

   - ECC: [here](https://github.com/PLATOONProject/market4.0-execution_core_container_business_logic/tree/platoon_uc) (platoon_uc branch)
   - Data App: (platoon_uc branch)
   - Dependencies:
     * Multipart Message Processor (main branch): [here](https://github.com/PLATOONProject/market4.0-ids_multipart_message_processor)
     * Websocket Message Streamer (main branch): [here](https://github.com/PLATOONProject/market4.0-websocket_message_streamer)

#### Build dependencies
First of all, dependencies repositories above must be cloned and built, by placing in their own repository folder and running Maven install (Maven is required):

 ```mvn install```

The generated Jars should have been placed in the Maven repository folder. These will be used by commands below during building phases (since are defined as dependencies in the relative POM files).

#### Build Data App and ECC JARs
Once the jars has been installed in the Maven repository for both dependencies, you can build Data App and ECC jars:

```mvn package```  (repeat for each repository folder)

The Jars generated in each `target` folder, will be used by respective Dockerfile when building the images.

#### Build Docker images
Put in each repository folder and run (Docker must be installed):

```docker build --tag platoon/ecc```
```docker build --tag platoon/data_app```


**Note.** By using same images name as the ones defined in the services part of provided docker-compose, you can still use that file to run `docker-compose up`. Docker will use the locally built images instead of pulling the ones of Docker Hub.
