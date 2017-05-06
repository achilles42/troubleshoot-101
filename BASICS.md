VIM SHORTCUT
```
* :%s/\s\+$//g
* :%s/^\s\+//g
```

Note:  s for space, ^ from beginning, $ for end of line, + for more than one.


TCPDUMP:
* tcpdump -D
check all the available interfaces

* tcpdump -i eth0
Listen on eth0 interface

* tcpdump -n
To display IP address and port instead on domain name

* tcpdump -n dst host 192.168.1.1
Captures packet whose destination IP is 192.158.1.1

* tcpdump -n src host 192.168.1.1

* tcpdump -n host 192.168.1.1
src/dst anything is 192.168.1.1

* tcpdump -n dst net 192.168.1.0/24
* tcpdump -n src net 192.168.1.0/24
* tcpdump -n "dst host 192.168.1.1 and dst port 22"
* tcpdump -n "dst host 192.168.1.1 and (dst port 80 or dst port 443)"
* tcpdump -v icmp
References: http://rationallyparanoid.com/articles/tcpdump.html

* knife exec -E 'nodes.transform ("name:node_name") {|n| n.run_list.remove ("recipe[base]"); old_rl = n.run_list.to_a; puts n.run_list(["recipe[base_new]"] + old_rl); n.save }'

--> ETCD url for creating etcd entry for grpc_healthcheck_agent:
    * curl -L http://etcd_ip:2379/v2/keys/app_name/version -XPUT -d value=4
    * curl http://apt_repo_ip:8080/api/repos/apt_repo/packages\?q\=aap-name | jq '' | grep app-name |  awk '{print $3}' | sort -n

--> http://www.ec2instances.info/?region=ap-southeast-1&selected=m4.large,c4.xlarge

--> Datadog
    * sudo /etc/init.d/datadog-agent restart
    * sudo /etc/init.d/datadog-agent info
