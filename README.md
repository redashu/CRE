# CRE

## Podman : THe most awesome container runtime

## Info about POdman 

<ul>
    <li>deamon less </li>
    <li> /var/lib/containers/ is the default roots </li>
    <li> can search registries automatically </li>
    <li> from a running container we can generate kubernetes pod YAML </li>
</ul>

## Installing POdman in RHEL 8

```
dnf install podman  -y
```

## configuration details 

```
[root@ip-172-31-6-97 containers]# cd /etc/containers/
[root@ip-172-31-6-97 containers]# 
[root@ip-172-31-6-97 containers]# ls
certs.d  oci  policy.json  registries.conf  registries.conf.d  registries.d  storage.conf
[root@ip-172-31-6-97 containers]# 


```

## document root

```
[root@ip-172-31-6-97 containers]# cd /var/lib/containers/
[root@ip-172-31-6-97 containers]# ls
sigstore  storage

```

## Container operations 

### pulling images 

```
podman pull alpine
```

### creating container 

```
root@ip-172-31-6-97 containers]# podman run -tid --name x2 -p 1223:80 nginx 
Resolved "nginx" as an alias (/var/cache/containers/short-name-aliases.conf)
Trying to pull docker.io/library/nginx:latest...
Getting image source signatures
Copying blob b82f7f888feb done  
Copying blob b21fed559b9f done  
Copying blob 03e6a2452751 done  
Copying blob edb81c9bc1f5 done  
Copying blob 5430e98eba64 done  
Copying blob b4d181a07f80 done  
Copying config 4f380adfc1 done  
Writing manifest to image destination
Storing signatures
d02f40c2912414c8bf7799bb0c5d96096b35c6b257310a42233dcc98ffc38003

```

### Build and cache containers location

```
[root@ip-172-31-6-97 containers]# cd /var/cache/containers/
[root@ip-172-31-6-97 containers]# ls
short-name-aliases.conf  short-name-aliases.conf.lock

```

### generating POD yaml from a running container 

```
[root@ip-172-31-6-97 ~]# podman ps
CONTAINER ID  IMAGE                                     COMMAND               CREATED             STATUS                 PORTS                 NAMES
c5f604926d41  registry.fedoraproject.org/fedora:latest  /bin/bash             12 minutes ago      Up 12 minutes ago                            x1
d02f40c29124  docker.io/library/nginx:latest            nginx -g daemon o...  About a minute ago  Up About a minute ago  0.0.0.0:1223->80/tcp  x2

---

[root@ip-172-31-6-97 ~]# podman generate kube x2
# Generation of Kubernetes YAML is still under development!
#
# Save the output of this file and use kubectl create -f to import
# it into Kubernetes.
#
# Created with podman-3.0.2-dev
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: "2021-07-03T07:06:43Z"
  labels:
    app: x2
  name: x2
spec:
  containers:
  - args:

```

### saving yaml file 

```
root@ip-172-31-6-97 ~]# podman generate kube x2  >nginx.yml
[root@ip-172-31-6-97 ~]# 
[root@ip-172-31-6-97 ~]# 
[root@ip-172-31-6-97 ~]# ls
anaconda-ks.cfg  nginx.yml  original-ks.cfg

```
### generating POD and service both 

```
podman generate kube  x2  -s

```


