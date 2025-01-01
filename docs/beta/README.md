---
icon: simple/nginx
title: readme.md
---

# README (Beta)

This is to simulate on how `Nginx` can be used as load-balancer and web backend with Multipass.

## Prerequisites

Below are the [cloud_init files](https://github.com/myseq/lab21/docs/beta/) needed.

|        | Files            | Description                   |
| -----: | :--------------- | :---------------------------- |
|  1     | `ci_backend1.yaml` | Setup Nginx web server and simulate the backend server_1 |
|  2     | `ci_backend2.yaml` | Setup Nginx web server and simulate the backend server_2 |
|  3     | `ci_backend3.yaml` | Setup Nginx web server and simulate the backend server_3 |
|  4     | `ci_lbr.yaml`      | Setup Nginx web server and work as the load balancer     |

With the YAML files, it took me less than 8 min to setup the all the 4 servers.

## Tutorial

We will create 4 instances of servers with 1 being the reverse-proxy and load-balancer.
The other 3 will act as backend web server.

Here's the simple architecture:

```console
Load-Balancer (LBR)
├── backend1                # Backend server_1
├── backend2                # Backend server_2
└── backend3                # Backend server_3
```

### Create LBR

```console
PS> multipass launch -n lbr --cloud-init ci_lbr.yaml
```

### Create 3 Backend Servers

```console
PS> multipass launch -n backend1 --cloud-init ci_backend1.yaml
PS> multipass launch -n backend2 --cloud-init ci_backend2.yaml
PS> multipass launch -n backend3 --cloud-init ci_backend3.yaml
```

### Testing

Open browser at <http://lbr.mshome.net/> and *keep refresh*.

## Links 

 - See [Load-Balance with Nginx](https://myseq.blogspot.com/2022/10/load-balance-with-nginx.html).


