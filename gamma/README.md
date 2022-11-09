# README.md
This is an interesting tutorial that focus on how operation works in IaaS.  

This tutorial will include:
1. Using docker and k8s cluster.
2. Creating 4 VM which include:
   1. K8s client that runs kubctl remotely 
   2. K8s master node
   3. 2 worker nodes for load-balancing app/jobs
3. Perform patching operation on mutable infrastructure
   1. Deploy the new image with k8s
   2. Simulate patching operation on apps
4. Develop a utility to check and monitor the patch operation
   - webping.py

