###Advanced Clustering
Linux Clastering includes many advanced techniques to cover all types of Cluster. In the section [Cluster Basics](https://github.com/kalise/Linux-Tutorial/blob/master/content/cluster-basics.md) we setup a simple Active/Standby cluster. In this section, we are going to extend our Cluster to become an Active/Active cluster.

In an Active/Standby cluster, the standby node is doing nothing for most of the time. Since we do not have shared data between the two nodes, there is no risk of data corruption, the second node can partecipate to the cluster task becoming an active member and improving the performances of the whole cluster. To achieve this goals, we make the HTTP Server running on both the nodes and installing a Load Balancer on both the nodes to distribute the client's requests in a Round Robin fashion.

![](../img/active-active-cluster.jpg?raw=true)

Remove the HTTP Server resource from the Cluster

    [root@benji ~]# pcs resource delete HTTPServer
    Attempting to stop: HTTPServer...Stopped

Add back the HTTP Server resource by changing its type

    [root@benji ~]# pcs resource create httpd systemd:httpd \
    > configfile=/etc/httpd/conf/httpd.conf \
    > op monitor interval=30s clone

We changed from ``ocf:heartbeat:apache`` to ``systemd:httpd`` since we want the HTTP Server started as Systemd daemon. This permits to have the server running on both the nodes at same time. Please, note that service is still managed by Pacemaker and it should not be started by Systemd.

On both the nodes, install the Load Balancer. We'll use HAProxy for simplicity

    [root@benji ~]# yum install haproxy -y

Make sure the same configuration file is present on both the nodes

    [root@benji ~]# vi /etc/haproxy/haproxy.cfg
    #---------------------------------------------------------------------
    # Global settings
    #---------------------------------------------------------------------
    global
        log         127.0.0.1 local2
        chroot      /var/lib/haproxy
        pidfile     /var/run/haproxy.pid
        maxconn     4000
        user        haproxy
        group       haproxy
        daemon
        # turn on stats unix socket
        stats socket /var/lib/haproxy/stats
    #---------------------------------------------------------------------
    # Common defaults
    #---------------------------------------------------------------------
    defaults
        mode                    http
        log                     global
        option                  httplog
        option                  dontlognull
        option http-server-close
        option forwardfor       except 127.0.0.0/8
        option                  redispatch
        retries                 3
        timeout http-request    10s
        timeout queue           1m
        timeout connect         10s
        timeout client          1m
        timeout server          1m
        timeout http-keep-alive 10s
        timeout check           10s
        maxconn                 3000
    #---------------------------------------------------------------------
    # Listen configuration
    #---------------------------------------------------------------------
    listen apache
        bind 10.10.10.23:80 transparent #bind to the Virtual IP
        mode http
        option http-server-close
        option forwardfor
        balance roundrobin
        server holly 10.10.10.22:80 check
        server benji 10.10.10.24:80 check

Now add the HAProxy as resource to the Cluster. Since we wont the Load Balancer running on both the nodes to handle the client's requests, we'll configure it as Systemd daemon.

    [root@benji ~]# pcs resource create haproxy systemd:haproxy \
    > op monitor interval=15s clone


