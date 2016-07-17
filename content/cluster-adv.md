###Advanced Clustering
Linux Clastering includes many advanced techniques to cover all types of Cluster. In the section [Cluster Basics](https://github.com/kalise/Linux-Tutorial/blob/master/content/cluster-basics.md) we setup a simple Active/Standby cluster. In this section, we are going to extend our Cluster to become an Active/Active cluster.

In an Active/Standby cluster, the standby node is doing nothing for most of the time. Since we do not have shared data between the two nodes, there is no risk of data corruption. The second node can partecipate to the cluster task becoming an active member and improving the performances of the whole cluster. To achieve this goals, we make the HTTP Server running on both the nodes and installing a Load Balancer on both the nodes to distribute the client's requests in a Round Robin fashion.

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

Restart the cluster 

    [root@benji ~]# pcs cluster start --all
    holly: Starting Cluster...
    benji: Starting Cluster...

and check the status

    [root@benji ~]# pcs status
    Cluster name: mycluster
    Last updated: Mon Jul 18 01:06:01 2016 
    Stack: corosync
    Current DC: holly.b-cloud.it (version 1.1.13-10.el7_2.2-44eb2dd) - partition with quorum
    2 nodes and 5 resources configured
    Online: [ benji holly ]
    Full list of resources:
        VIP-10.10.10.23        (ocf::heartbeat:IPaddr2):       Started benji
        Clone Set: httpd-clone [httpd]
             Started: [ benji holly ]
        Clone Set: haproxy-clone [haproxy]
             Started: [ benji holly ]
        PCSD Status:
          holly: Online
          benji: Online
        Daemon Status:
          corosync: active/enabled
          pacemaker: active/enabled
          pcsd: active/enabled

The Cluster is running with HTTP Server and HAProxy running on both the nodes. Check the services are started in Systemd fashion

    [root@benji ~]# systemctl status haproxy
    ● haproxy.service - Cluster Controlled haproxy
       Loaded: loaded (/usr/lib/systemd/system/haproxy.service; disabled; vendor preset: disabled)
      Drop-In: /run/systemd/system/haproxy.service.d
               └─50-pacemaker.conf
       Active: active (running) since Mon 2016-07-18 01:05:55 CEST; 5min ago
     Main PID: 1748 (haproxy-systemd)
       CGroup: /system.slice/haproxy.service
               ├─1748 /usr/sbin/haproxy-systemd-wrapper -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid
               ├─1749 /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid -Ds
               └─1750 /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg -p /run/haproxy.pid -Ds
    Jul 18 01:05:55 benji systemd[1]: Started Cluster Controlled haproxy.
    Jul 18 01:05:55 benji systemd[1]: Starting Cluster Controlled haproxy...

and

    [root@benji ~]# systemctl status httpd
    ● httpd.service - Cluster Controlled httpd
       Loaded: loaded (/usr/lib/systemd/system/httpd.service; disabled; vendor preset: disabled)
      Drop-In: /run/systemd/system/httpd.service.d
               └─50-pacemaker.conf
       Active: active (running) since Mon 2016-07-18 01:05:53 CEST; 6min ago
         Docs: man:httpd(8)
               man:apachectl(8)
     Main PID: 1729 (httpd)
       Status: "Total requests: 0; Current requests/sec: 0; Current traffic:   0 B/sec"
       CGroup: /system.slice/httpd.service
               ├─1729 /usr/sbin/httpd -DFOREGROUND
               └─1734 /usr/sbin/httpd -DFOREGROUND
    Jul 18 01:05:52 benji systemd[1]: Starting Cluster Controlled httpd...
    Jul 18 01:05:53 benji systemd[1]: Started Cluster Controlled httpd.
