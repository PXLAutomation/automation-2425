# Test Vagrant and Ansible set-up
## Provision machine
```bash
vagrant up
```
Output:
```bash
Bringing machine 'server0' up with 'virtualbox' provider...
Bringing machine 'server1' up with 'virtualbox' provider...
[...]
==> server0: PXL Lab machine build complete.
[...]
==> server1: PXL Lab machine build complete.
```

## clean previous localhost ssh key references
make sure the script is executable
```bash
chmod +x ./clean_known_hosts
```
run the cleaning script (it is idempotent)
```bash
./clean_known_hosts
```
Typical output:
```
# Host [127.0.0.1]:2222 found: line 11
/home/student/.ssh/known_hosts updated.
Original contents retained as /home/student/.ssh/known_hosts.old
# Host [127.0.0.1]:2200 found: line 10
/home/student/.ssh/known_hosts updated.
Original contents retained as /home/student/.ssh/known_hosts.old
```
or:
```
Host [127.0.0.1]:2222 not found in /home/tomc/.ssh/known_hosts
Host [127.0.0.1]:2200 not found in /home/tomc/.ssh/known_hosts
```

## Test network connectivity
```bash
vagrant ssh server0
[vagrant@server0 ~]$ ping server1
PING server1 (192.168.56.101) 56(84) bytes of data.
64 bytes from server1 (192.168.56.101): icmp_seq=1 ttl=64 time=0.794 ms
64 bytes from server1 (192.168.56.101): icmp_seq=2 ttl=64 time=0.371 ms
^C
[vagrant@server0 ~]$ exit
```

## Test ansible
```bash
ansible servers -i inventory -m ping
```
Output:
```
server0.pxldemo.local | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
server1.pxldemo.local | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

## Remove machines
```bash
vagrant destroy -f
```
Output:
```
==> server1: Forcing shutdown of VM...
==> server1: Destroying VM and associated drives...
==> server0: Forcing shutdown of VM...
==> server0: Destroying VM and associated drives...
```

## Check dangling machines
```
vagrant global-status --prune
```
Output:
```
id       name   provider state  directory
--------------------------------------------------------------------
There are no active Vagrant environments on this computer! Or,
you haven't destroyed and recreated Vagrant environments that were
started with an older version of Vagrant.
```