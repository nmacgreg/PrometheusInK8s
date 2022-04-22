# A Playbook for installing Kubernetes on CentOS 7

* Who:  Neil
* What: This is a playbook for installing and configuring a particular version of Kubernetes on CentOS 7
* When: Jan - Apr, 2022
* Where: 
    * Ref: [My notes from the course](https://github.com/ualbertalib/Cloud_Study_Notes/blob/main/docs/Monitoring/ACGMonitoring.md)
    * Ref: [Eerily similar, but for Ubuntu](https://github.com/nmacgreg/Intro2k8s)
    * Ref: This is a companion to ["Monitoring Kubernetes with Prometheus"](https://learn.acloud.guru/course/97037e05-88ed-41a1-92ee-f5a8080318c2/dashboard)... you can't take the course without having a working instance of K8s
* Why: 
    * These courses demand that you have k8s running, but getting it running is non-trivial!

## Making Use of this Playbook

* Assuming that you are taking the course & using their "Cloud Playground",
* After you create the servers, 
    * From the ACG web page, get the IP addresses for the "master" and "worker" nodes & define those name in your /etc/hosts file!
    * manually login with the given credentials, accept key, and change the password, then logout
    * Use this to upload your key: ```ssh-copy-id -i ~/.ssh/id_ecdsa.pub cloud_user@master```
    * Repeat for the worker node
    * To allow direct access to SSH as root: 
	* become root (needs pw) ```sudo su - ```
	* copy your public key into root's ~/.ssh/authorized_keys (sorry, no convenient way to do this)
* (You might test that you can ```ssh cloud_user@master``` & ```ssh cloud_user@worker``` without a password) -- now you're set up to use this Ansible playbook!

## How to run this playbook 

* Initial checking, to see if you got all the uid/pw/keys done correctly: 

```
ansible-playbook -i inventory k8sCentOS.yml  --check
```

* A real run in look like: 

```
ansible-playbook -i inventory k8sCentOS.yml
```

## Tangents

* In ITS's "ansible-config/" tree, I already built [ansible-config/projects/microk8s-cluster/](https://github.com/ualbertalib/ansible-config/tree/main/projects/microk8s-cluster)
    * That playbook builds out MicroKubernetes on CentOS7... Microk8s *comes with Prometheus running by default!* 
    * ... but @ April, 2022 I haven't had time to play with Prometheus (can't figure out how to ingress into it)!
* Impression: running k8s in the cloud is highly dependent on a magical front-end -- the load balancer
    * Sometimes it's "Traefik"
    * Sometimes is's HAProxy (probably a newer, more flexible version than ours)
    * I think this is referred to as the "Ingress Controller" (?)
    * It's reponsible for all external communications with applications running inside the cluster
    * I think that means it's running inside a Docker container on the nodes of your cluster, but *outside* of the cluster itself(?), but I'm not certain on this point
    * (But, to my knowledge, HAProxy only scales to 2 nodes... how do they managing to run it on more than 2 nodes?)
