vagrant-kubernetes
=====================
A vagrant configuration to setup Kubernetes in a single node through Ansible.   
The last version available in Kubernetes repo will be installed.

## Deploy 
```
vagrant up
```

## Tips 
Make sure that you have kubectl installed in your machine. If yes, the playbook will setup the cluster in kubectl config.   
It is recommend to configure the hosts of your machine to resolve the ip of the cluster. For that, add the entry below in /etc/hosts.
```
10.0.10.11 dev.local
```

## TODO
GitOps strategy
