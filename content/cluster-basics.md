###Cluster Basics
A cluster is two or more computers (cluster members) that work together to perform a task, for example, provide high availability of a given service. High availability clusters provide highly available services by eliminating single points of failure and by failing over services from one cluster member to another in case a node becomes inoperative.

Typically, services in a high availability cluster maintain data integrity as one cluster member takes over control of a service from another cluster member. Node failures in a high availability cluster are not visible from clients outside the cluster.

In the Linux world, there are many cluster tools. The most used is **Pacemaker**. A cluster configured with Pacemaker comprises separate component daemons that monitor cluster membership, scripts that manage the services, and resource management subsystems that monitor the resources. The following components form the Pacemaker architecture:

1. **Cluster Information Base**: the Pacemaker information daemon distributes and synchronizes the cluster configuration and status information from the Designated Coordinator (DC) of the cluster to all other cluster members. The DC is one cluster member designated to store the cluster state.
2. **Cluster Resource Management Daemon**: cluster resources managed by this component can be queried by client systems, moved, instantiated, and changed when needed. Each cluster node also includes a local resource manager daemon that acts as an interface between Cluster Resource Manager daemon and the resource itself. The local resource manager passes commands from Cluster Resource Manager to agents, such as starting and stopping and relaying resurce status information.
3. **Fencing Manager**: often deployed in conjunction with a power supply switch, this component acts as a cluster resource in Pacemaker that processes fence requests, forcefully powering down nodes and removing them from the cluster to ensure data integrity.

####Install and Configure Pacemaker
We are going to setup a simple HA Cluster based on Pacemaker as following. This example will be also used to explain the basic concepts of Linux Clustering. 

                                      |
    +----------------------+          |          +----------------------+
    | Node01               |          |          | Node02               |
    | holly.noverit.com    +----------+----------+ benji.noverit.com    |
    | 10.10.10.22          |                     | 10.10.10.24          |
    +----------------------+                     +----------------------+

Install, start and enable Pacemaker on both the nodes

    [root@holly ~]# yum -y install pacemaker pcs
    [root@holly ~]# systemctl start pcsd
    [root@holly ~]# systemctl enable pcsd
    [root@holly ~]# passwd hacluster
    Changing password for user hacluster

    [root@benji ~]# yum -y install pacemaker pcs
    [root@benji ~]# systemctl start pcsd
    [root@benji ~]# systemctl enable pcsd
    [root@benji ~]# passwd hacluster
    Changing password for user hacluster

Pacemaker need to communicate beween nodes, enable the port firewall on each node, which by default is 2224 over TCP. Otherwise, disable the firewall if you are working in a secure setup.

    [root@holly ~]# systemctl stop firewalld
    [root@holly ~]# systemctl disable firewalld
    [root@benji ~]# systemctl stop firewalld
    [root@benji ~]# systemctl disable firewalld

Only on a node of the cluster, configure the cluster

    [root@holly ~]# pcs cluster auth holly benji
    Username: hacluster
    Password:
    holly: Authorized
    benji: Authorized
    [root@holly ~]# pcs cluster setup --name mycluster holly benji
    Shutting down pacemaker/corosync services...
    Redirecting to /bin/systemctl stop  pacemaker.service
    Redirecting to /bin/systemctl stop  corosync.service
    Killing any remaining services...
    Removing all cluster configuration files...
    holly: Succeeded
    benji: Succeeded
    Synchronizing pcsd certificates on nodes holly, benji...
    benji: Success
    holly: Success
    Restaring pcsd on the nodes in order to reload the certificates...
    benji: Success
    holly: Success

On the same node, start and enable the cluster

    [root@holly ~]# pcs cluster start --all
    benji: Starting Cluster...
    holly: Starting Cluster...
    [root@holly ~]# pcs cluster enable --all
    holly: Cluster Enabled
    benji: Cluster Enabled


Check the status of the cluster

    [root@holly ~]# pcs status
    Cluster name: mycluster
    WARNING: no stonith devices and stonith-enabled is not false
    Last updated: Sat Jul 16 17:20:14 2016
    Stack: corosync
    Current DC: holly (version 1.1.13-10.el7_2.2-44eb2dd) - partition with quorum
    2 nodes and 0 resources configured
    Online: [ benji holly ]
    Full list of resources:
      PCSD Status:
        holly: Online
        benji: Online
    Daemon Status:
        corosync: active/enabled
        pacemaker: active/enabled
        pcsd: active/enabled



