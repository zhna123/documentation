# Storm and Parallelism settings
With a recent issue to generate parallelism settings on each bolt, I had a clearer understanding of some concepts.

### 1. Terminologies

**Nimbus**

* Master node of storm cluster (like the job tracker in hadoop)
* Distribute code and assign tasks and monitor failures

**Worker node (Supervisor)**

* Each worker node runs a daemon called Supervisor. 
* Listen for work assigned to its machine and starts and stops worker processes as needed

**Worker process**

* A worker node can run multiple worker process
* A worker process execute a subset of a topology, and runs in its own JVM

**Executors (threads)**

* A single worker process can run multiple executors
* Each executor is a thread spawed by the worker process
* Each executor runs one or more tasks of the same component (spout or bolt)

**Tasks**

* A task performs the actual data processing

**Zookeeper**

* A service used by a cluster to coordinate between themselves and maintaining shared data with robust synchronization techniques.

* All coordination between Nimbus and the Supervisors is done through a Zookeeper cluster. 

* Additionally, the Nimbus daemon and Supervisor daemons are fail-fast and stateless; all state is kept in Zookeeper or on local disk. This means you can `kill -9` Nimbus or the Supervisors and they'll start back up like nothing happened. This design leads to Storm clusters being incredibly stable.

### 2. config the parallelism settings [1]

parallelism setting is used to set the initial number of executors (threads) of a component. 

Some related settings:

* **config#setNumWorkers** (this set the number of worker processes for the topology accorss machines in the cluster)
* **topologyBuilder#setSpouts()**   (nubmer of executors. set how many executors per spout)
* **topologyBuilder#setBolt()**   (nubmer of executors. set how many executors per bolt)
* **componentConfigurationDeclarer#setNumTasks()**   (number of tasks to create per component)

**Recommended settings for a uniform distributed workload:**

* set #workers to a multiple of #machines
* set parallelism to a multiple of #workers
* set #tasks to a multiple of the parallelism 

[1] http://www.michael-noll.com/blog/2012/10/16/understanding-the-parallelism-of-a-storm-topology/

