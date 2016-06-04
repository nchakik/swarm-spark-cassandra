
# Swarm Spark Cassandra cluster deployment scripts


Apache Spark in a Docker container, based on `java:8`. Heavily inspired by [gettyimages/docker-spark](https://github.com/gettyimages/docker-spark). Cluster deployed with Docker Compose and orchestrated with Docker Swarm.

Official image for [Cassandra](https://hub.docker.com/_/cassandra/) is used and installed on the same nodes as Spark slaves.

### Run the cluster locally on VirtualBox

In `scripts/`, run :

	./bootstrap.sh

This script will set up a key-value store using Consul, a Swarm cluster with a master and a slave, and an overlay network for multi-host networking.

### After creating the nodes

Connect to the Swarm master with:

	eval $(docker-machine env --swarm swarm-master)

The cluster configuration should be visible by running:

	docker info

## Deploy Spark

Deploy the containers on constrained nodes with:

	docker-compose --x-networking --x-network-driver=overlay up -d

## Deploy Cassandra

In `scripts/`, run:

	./deploy_cassandra.sh
	
After deployment is complete, you can run `cqlsh` this way:

    docker run -it --rm --net container:cass1 cassandra:2.2.4 cqlsh
    
A folder `/data` is mounted as a shared volume outside of the container to save Cassandra data.  

## References

- Multi-host networking: https://docs.docker.com/engine/userguide/networking/get-started-overlay/
- Networking in Compose: https://docs.docker.com/compose/networking/
