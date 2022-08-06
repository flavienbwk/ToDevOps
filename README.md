# ToDevOps

A basic thus state-of-the-art architecture to start your modern DevOps stack learning.

![ToDevOps architecture schema](./schema.jpg)

## Stack

- Terraform : instanciate 3 VMs on Scaleway
- Ansible : install and configure those VMs (K8S cluster, GitLab, API...)
- Kubernetes : Softwares orchestration
  - Ingress : Cluster endpoint using domains
  - Linkerd : Service mesh for monitoring network & system metrics
  - Fluentd : Operator for capturing apps logs
  - ArgoCD : Infrastructure as Code (IaC) tool to fetch your apps
- GitLab : registries, runners (CI/CD)
- ReactJS Flask [boilerplate](https://github.com/flavienbwk/reactjs-flask-ldap-boilerplate) : an example of micro-services project

Basically, we're going to get our _boilerplate_ continuously deployed by _ArgoCD_ from a _GitLab_ repository in a _Kubernetes_ cluster of 3 baremetal _Ansible_-installed nodes on _Terraform_-instanciated VMs. _Linkerd_ will monitor network issues and _Fluentd_ capture containers logs into an Elasticsearch instance.

## Deploy

This section explains how to run this infrastructure from your local computer.

I assume you have [Terraform](https://www.terraform.io/downloads) and [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) installed and ready on your computer.

### 1. Instanciating the infrastructure

We'll use [Scaleway](https://www.scaleway.com/en/) as a cloud provider. We recommend at least 2 VMs with 4Gb RAM (`k8s-nodes`) and 1 VM with 8Gb RAM (`k8s-master`).

1. Go to your Scaleway account > [Credentials](https://console.scaleway.com/project/credentials) and create a new API key `ToDevOps`

2. Run the following `export` commands replacing values by yours

    ```bash
    cd ./plans

    export TF_VAR_SCW_PROJECT_ID="my-project-id"
    export TF_VAR_SCW_ACCESS_KEY="my-access-key"
    export TF_VAR_SCW_SECRET_KEY="my-secret-key"
    ```

3. Make sure there's no error by running init and plan commands

    ```bash
    terraform init
    terraform plan
    ```

4. Execute the plan

  ```bash
  terraform apply
  ```

5. Retrieve output values for our Ansible inventory file

  ```bash
  terraform output -json > terraform_values.json
  ```

6. Edit values of our Ansible inventory file from Terraform output values

  ```bash
  cd .. # Go back to root of this repo
  sudo apt install -y jq # Used to parse values
  bash terraform_to_ansible_values.sh
  ```

### 2. Deploying platform

This step is about deploying our Kubernetes cluster and its different services as well as GitLab.

1. Make sure `./inventories/scaleway.ini` values are valid

## Why this repo ?

As I trust DevOps for being able to deeply transform small or big organizations in order to deliver quickly and more reliably, I am currently (2022) transitionning from software to DevOps engineering. This repository compiles my current knowledge on which and how DevOps technologies can be deployed to allow an IT team to work efficiently.

## Found this repo useful ?

Please consider leaving a **star**, sharing improvements with **pull requests** or [**sponsoring**](https://github.com/sponsors/flavienbwk) me.

## More resources for your career in DevOps

- https://sre.google/books
- https://www.google.com/books/edition/Building_Secure_and_Reliable_Systems/Kn7UxwEACAAJ
- https://www.coursera.org/professional-certificates/sre-devops-engineer-google-cloud
- https://www.cloudskillsboost.google/quests/141?locale=fr
