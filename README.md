# ansible-setup
This repository provides an Ansible playbook to automate the installation and configuration of essential tools and dependencies.

This will help you quickly set up a newly created server, install utilities, and protect against known attacks. From the description, you can learn how to deploy any of the applications on the VPS server.

# What attacks did we face?

# 1. Docker bruteforce
   
In test mode, the infrastructure was deployed in the simplest way, and of course with the database port forwarded. If you have a small project on one server, never open ports for databases.

## How the attack works?

The program searches for open database ports - 5432, 27017 and others. Then it tries to guess the password. When the password is guessed (it is unlikely that you make it very complex) - the attack begins. A process appears in the container, we had it called kdevtmpfsi. The malware itself is a bitcoin miner and will load your server to the maximum (docker in non-swarm mode does not limit resources for containers). In addition to terrible performance, you will most likely get broken database data.

The solution to this problem in the case of docker is simple - stop and delete the container and its volume. If you encountered this already in production - you can try to kill the cron job tasks and the process. This will stop the malware, but the data may already be corrupted.

You should never use open ports with a soft password in testing and production - this will lead to big problems with the server. All you need to do for security is remove "ports: 5432:5432" and you are safe from this attack!

## 2. SSH-bruteforce

An attack with a similar principle but much worse consequences
