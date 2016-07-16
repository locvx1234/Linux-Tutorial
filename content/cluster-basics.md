###Cluster Basics
A cluster is two or more computers (cluster members) that work together to perform a task, for example, provide high availability of a given service. High availability clusters provide highly available services by eliminating single points of failure and by failing over services from one cluster member to another in case a node becomes inoperative.

Typically, services in a high availability cluster maintain data integrity as one cluster member takes over control of a service from another cluster member. Node failures in a high availability cluster are not visible from clients outside the cluster.

In the Linux world, there are many cluster tools. The most used is **Pacemaker**. A cluster configured with Pacemaker comprises separate component daemons that monitor cluster membership, scripts that manage the services, and resource management subsystems that monitor the resources. The following components form the Pacemaker architecture:

1. **Cluster Information Base**: the Pacemaker information daemon distributes and synchronizes the cluster configuration and status information from the Designated Coordinator (DC) of the cluster to all other cluster members. The DC is one cluster member designated to store the cluster state.
2. **Cluster Resource Management Daemon**: cluster resources managed by this component can be queried by client systems, moved, instantiated, and changed when needed. Each cluster node also includes a local resource manager daemon that acts as an interface between Cluster Resource Manager daemon and the resource itself. The local resource manager passes commands from Cluster Resource Manager to agents, such as starting and stopping and relaying resurce status information.
3. **Fencing Manager**: often deployed in conjunction with a power supply switch, this component acts as a cluster resource in Pacemaker that processes fence requests, forcefully powering down nodes and removing them from the cluster to ensure data integrity.

