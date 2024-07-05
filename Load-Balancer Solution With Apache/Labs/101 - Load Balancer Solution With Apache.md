# 101 - Load Balancer Solution With Apache
Let us take a look at the updated solution architecture with an LB added on top of Web Servers (for simplicity let us assume it is a software L7 Application LB, for example - Apache, NGINX or HAProxy)
<img width="571" alt="image" src="https://github.com/gideonsngo/DevOpsTraining/assets/74353147/8ea2e113-56d4-4322-90c8-d8454d5daef2">

In this project we will enhance our Tooling Website solution by adding a Load Balancer to disctribute traffic between Web Servers and allow users to access our website using a single URL.

## Task
Deploy and configure an Apache Load Balancer for Tooling Website solution on a separate Ubuntu EC2 intance. Make sure that users can be served by Web servers through the Load Balancer.

To simplify, let us implement this solution with 2 Web Servers, the approach will be the same for 3 and more Web Servers. NGINX or HAProxy)

