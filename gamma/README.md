# README.md
This is an interesting tutorial that focus on how operation works in IaaS.  

This tutorial will experience in:
- create VM hosts with multipass
- create custom docker image
- create k8s clueter that load-balance a Nginx web app
- create a utility to inspect and monitor the patch operation
- simulate docker patching on mutable infrastructure

## Architecture
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


## Plan (high level) 
1. Creating 4 VM which include: 
   - 1 k8s client, 1 k8s master node, and 2 k8s worker node
2. Develop a utility to check and monitor the patch operation
   - webping.py
3. Perform patching operation on mutable infrastructure
   - Deploy the new image with k8s
   - Simulate patching operation on apps

