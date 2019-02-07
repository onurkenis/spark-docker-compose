# Docker compose for spawning on demand HDFS and Spark clusters

# Build the required Docker images
`./build-images.sh`

# Run the cluster
Note: Run below commands from the directory where `docker-compose.yml` file is present.
## bring up the cluster
`docker-compose up -d`
## to scale HDFS datanode or Spark worker containers
`docker-compose scale spark-slave=n` where n is the new number of containers.

## Sample application lunch with spark-submit
  - Copy sample jar file from host to container
    * `docker cp test-spark.jar CONTAINER_ID:/test-spark.jar`
    
  - Run commands in running spark-master container
    * `docker exec -i -t spark-master /bin/bash`    

  - Create directory on hdfs and copy sample jar into the created directory
    * `hdfs dfs -mkdir /app`   
    * `hdfs dfs -put /test-spark.jar /app`   

  - Run spark-submit
    * `cd /sbin`   
    * `spark-submit --class com.onurkenis.app.App --master spark://spark-master-host:7077 --deploy-mode cluster        hdfs:///app/test-spark.jar`
