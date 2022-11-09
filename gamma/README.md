# README.md
This is an interesting tutorial that focus on how operation works in IaaS.  

This tutorial will experience in:
- create VM hosts (with multipass)
- create custom docker image (with docker compose)
- create k8s clueter that load-balance an app (Nginx web)
- create a utility to inspect and monitor the patch operation (webping.py)
- **simulate docker patching on mutable infrastructure** :sparkler: 

## Architecture and Plan
```
             -------------------------------------------
             |                       ______            |
             |                |-----| mino |           |
 ______      |   ______       |      ======            |
| kiko | ------ | lilo | ---- |         k8s worker1    |
 ======      |   ======       |      ______            |
    kubectl  |    k8s master  |-----| nino |           |
             |                       ======            |
             |                          k8s worker2    |
             |                                         | 
             ----------- k8s cluster -------------------

```

|     | VM Host | Role              |
| --: | :------ | :---------------- |
| 1   | kiko    | K8s client        |
| 2   | lilo    | K8s master node   |
| 3   | mino    | K8s worker node 1 |
| 4   | nino    | K8s worker node 2 |


### Plan (high level) 
1. Creating 4 VM which include: 
   - 1 k8s client, 1 k8s master node, and 2 k8s worker node
2. Deploy unpatch app at k8s clueter with load-balancer
3. Develop a utility to check and monitor the patch operation
   - webping.py
4. Perform patching operation on mutable infrastructure
   - Deploy the new image with k8s
   - Simulate patching operation over unpatch app with patched app

## 1. Create 4 VM
- create 4 VM
- install and configure k8s client
- install and configure k8s master node
- install and configure k8s worker nodes
```console

```


## 2. Deploy Unpatch App
- create custom unpatch docker image (with label `unpatch`)
- create custom patched docker image (with label `patched`)
- deploy the `unpatch` docker image to k8s
```console

```


## 3. Develop webping Utility
- create webping with Python
- test and monitor `unpatch` docker runtime/container
```console

```


## 4. Perform Patching Operation
- test and monitor app (running on `unpatch` docker runtime/container)
- deploy the `patched` docker image over the `unpatch` container
- test and monitor app (running on `patched` docker runtime/container)
```console

```



