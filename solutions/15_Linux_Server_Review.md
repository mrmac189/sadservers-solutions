https://sadservers.com/scenario/linux-server-review
https://docs.sadservers.com/docs/scenario-guides/practical-linux-server-review/

The purpose of this scenario is to review a Linux server and be able to answer questions like:

- What's the purpose of the server?
- What's the hardware (CPU / RAM / disk / net) utilization of the server? is there a problem there?
- What is running and what's going on in the server?

## Objective 1: Characterize the Server 
```bash
ss -tulpn # OR netstat -tulpn
# webapp.py, postgres, haproxy
```
This is a Linux server, where Python web application is running. The app is using PostgreSQL as a database and Haproxy as a load balancer.

```bash
ps -ef # ps auxf for tree style
```
Same picture in process list. 

```bash
systemctl list-unit-files
systemctl list-units 
```
## Objective 2: Find the Server Hardware Utilization

### General
```bash
uptime
df -h # disk space
df -i # inodes
lsblk # disks 
mount 
free -m # vmstat 2 2 can ask 2 times with range of 2 secs
```
### Processes
```bash
top
htop
lscpu
```

### Out of memory
```bash
grep -i oom /var/log/messages
# oom-kill:constraint=CONSTRAINT_NONE,nodemask=(null),cpuset=containerd.service,mems_allowed=0,global_oom,task_memcg=/system.slice/cron.service,task=stress,pid=619,uid=0
sudo journalctl | grep -i "out of memory"
sudo dmesg | grep -i "out of memory"
# Out of memory: Killed process 619 (stress) 
sudo grep -E "oom|killed process" /var/log/syslog
```

### IO
```bash
iostat
sar -d 1 3
sudo iotop
```

### Network
```bash
ip a
ip -s link
# iptables -L # not found
# ifconfig # old UNIX command
sar -n DEV 1
```

## Objective 3: Explore the Applications
### HAProxy
```bash
ps auxf 
# haproxy is spawn from containerd
```
```bash
curl http://localhost:8000
# 56478

docker ps -a
docker inspect haproxy
# "Binds": [
# "/home/admin/haproxy.cfg:/usr/local/etc/haproxy/haproxy.cfg:rw"
# ],
```
### Webapp

```bash
ps -ef | grep py
vim  /home/admin/webapp.py
curl http://localhost:9000
```

### Postgres
```bash
sudo -s
su - postgres
psql -l
psql -d webapp
\d+
\d traffic
\q
```

## Objective 4: Review OS and Application Logs
```bash
journalctl -p err  # filter for error logs
journalctl -u webapp  # logs for the unit (service) webapp
journalctl -u postgresql  # postgres logs
docker logs haproxy
```
```bash
cat /var/log/messages
cat /var/log/syslog
cat /var/log/secure
```