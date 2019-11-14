# AWS VPC creation via Ansible - Scenario 1

This Repo contains the diagram, Ansible playbooks and relevant information to build an AWS VPC along with these elements:

- Security Groups
- NACL
- Subnets (Public and Private)
- IGW
- Route Table
- EC2 Instances


## Diagram
This is the diagram of the mini infraestructure created for this scenario:

![Diagram](https://github.com/carloshz4/aws_vpc1_cloudformation/blob/master/VPC1.jpg)


Note diagram was create with [draw.io](https://www.draw.io/) which is a handy and free tool to create and edit these kind of diagrams.

This [link](https://www.draw.io/?libs=aws2) loads directly the aws libraries.
And the desktop version can be downloaded [here](https://github.com/jgraph/drawio-desktop/releases/tag/v12.1.7)

## Playbooks
There are three Ansible playbooks in this repo:
```
vpc1.yml
vpc1-facts.yml
vpc1-rollback.yml

```
where:

#vpc1.yml
Contains the Ansible code to create the virtual architecture mentioned above

#vpc1-facts.yml
Contains the Ansible code to collect (facts) information about the elements created

#vpc1-rollback.yml
Contains the Ansible code to delete all elements created in the virtual architecture mentioned above


## Important notes
- The code was executed on the Ansible controller host as root user
- Root user has already the aws cli configured which means the .aws/credentials file should exist with the credentials like: 

    aws_access_key: 'AKIAIOSFODNN7EXAMPLE'
    aws_secret_key: 'wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY'

These credentials come from an aws privileged iam user 

- Because of the previous point, the tasks don't have reference to these credentials, but if needed they can be added to the tasks, like:
```
    - name: create vpc1
      ec2_vpc_net:
        aws_access_key: "AKIAIOSFODNN7EXAMPLE"
        aws_secret_key: "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
        name: "{{ vpc_name }}"
        cidr_block: "{{ vpcCidrBlock }}"
        region: "{{ region }}"
        dns_support: yes
        dns_hostnames: yes
        tenancy: default
        state: "{{ state }}"
      register: ec2_vpc_net_result
      tags: vpc
```
Just make sure not sharing these to the public.

- Executing the playbooks:

```
ansible-playbook vpc1.yml
ansible-playbook vpc1-facts.yml
ansible-playbook vpc1-rollback.yml
```


> Enjoy, make it different, make it better, and please share suggestions, ideas if you have.
