# Newstack_cdw Repository

This repo has scripts to spin up Newstack Demo in an VM or Bare-metal Ubuntu 20.04 lab

It currently includes Ansible, Kubernetes, PSO and PSO explorer.

Many thanks are due to Brian Kuebler & Chris Crow from Pure Storage for the original test drive idea, as well as all the Ansible yaml files.

### Requirements
You will need a Ubuntu 20.04 install.  

The pre-reqs for this environment are as follows

Server VM has to have at least 1 NIC connected to the Lan/Wan and 1 NIC connected to your iSCSI network

Access to at least 1 Pure FA running Purity 5.3 or higher (can be physical or VM)

Update/upgrade the environment using apt get
```
 sudo apt-get update
 sudo apt-get upgrade
 ```
Make sure ssh is installed and running

```
sudo service ssh status
```

If yes, skip to install git

if no

```
sudo apt-get install openssh-server
```

Enable the ssh service by typing

```
sudo systemctl enable ssh
```

Start the ssh service by typing
```
sudo systemctl start ssh
```

Install git

```
sudo apt install git
```
(best practice is to take a snapshot of your VM at this time, this will allow for quick restarts if something goes wrong or if you want a golden image to run demo from each time)


### Installing the demo

clone the demo repo with:
```git clone https://github.com/purechilab/newstack_cdw```

username :purechilab
password :Madne$$@99

```cd newstack_cdw```

Run the install_ubuntu.sh script
```./install_ubuntu.sh```

Note that this will also install all of the Ansible bits as well as the Pure PSO drivers and the new PSO explore bits

This will take between 8-15 minutes depending on environment. Great time for you to explain whats going on during the install. Feel free to ``` cat install_ubuntu.sh```  to walk through whats being done in the background

### Post install
 ```
 source ~/.bashrc
 kubectl get pods
 kubectl get nodes
 kubectl get svc
```
### Demos

The demo PSO demo scripts are located in the kubernetes_yaml directory. They are designed to be run in order as there may be dependancies.

There is also a MySQL demo that is located in the kubernetes_yaml/mysql_demo folder

Open the kubernetes_yaml/mysql_demo/example_commands.txt file for step by step instructions to follow for the mySQl demo

### PSO & EXPLORER Demos

```
kubectl get svc --namespace psoexpl -w pso-explorer
```

Jot down port number

open browser window with VM IP:Port number from above

PSO EXPLORER  will open

#### Create the PVC. Check that it is created on the Pure
```
kubectl apply -f 1_createPVC.yaml
```

#### Creates Minio Deployment
```
kubectl apply -f 2_minio.yaml

kubectl apply -f 3_service.yaml
```

You can now log in to minio using the service port. Find the port with the kubectl get svc (should always be 9000) command. http://<linuxIP>:<port> Username/password: minio:minio123

Continuing with the rest of the commands, which will take a snap and clone a new PVC from that snapshot

For a snap restore demo, you can scale to 0 replicas, restore the snap, and scale replicas to 1. The command to scale replicas is:

kubectl scale deploy minio-deployment --replicas=0

restore snap using step 6
start new minio instance step 7
create minio service step 8

```kubectl scale deploy minio-deployment --replicas=1```
Open new minio instance (default will be port 9001)




## Ansible Demo

The Full Ansible Demo requires access to 2 x FlashArrays

Demo 3 is an active cluster playbook. you can skip this if you only have 1 array

### Running the Demo

This demo allows the driver to run playbooks in the ansible_playbooks directory. They are numbered to run through a progression.

You can run each playbook with 'ansible-playbook -b <yaml file>'

The Active cluster demo requires access to two arrays (PSO Yaml file will need the info on both arrays)  

More notes to come..


# Additional customizations
initial requirements

- SSH server Running
- GIT installed
- Update/upgrade
Script will add the following
- installed multipath-tools
- installed python3-pip
- installed open-iscsi
