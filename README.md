## USAGE
When I started learning kubernetes, I found that all the deployment programs were too complicated and there were too many parameters to configure.

I want to make the configuration simple, and I can easily modify the configuration of the service. I think this is enough for testing.

This repo is mainly used to deploy the kubernetes cluster on centos7.
It is developed for self-test. (I developed this with reference to the kolla-ansible code.)

## How to use it

I recommend that you use source installation, which makes it easier to understand and troubleshoot problems.

1. You need to create a new folder for deployment.

```
mkdir ~/ansible-k8s-deploy
cd ~/ansible-k8s-deploy
```

2. Then download the source to this folder

```
ansible-k8s-deploy
└── ansible-k8s
```
3. Copy configuration file

```
cp -r ansible-k8s/etc/config-test/ .

ansible-k8s-deploy
├── ansible-k8s
└── config-test
```
4. You need to edit the configuration file:

```
config-test
├── globals.yml
└── multinode-inventory

1. You must set the k8s_api_server_vip in globals.yml
2. Please edit the multinode-inventory according to your node arrangement.
```

5. Node initialization

```
common:
yum install epel-release -y
yum install python-pip -y
yum install -y python-devel libffi-devel gcc openssl-devel git

deploy node:
pip install ansible
ps:Ensure that the deployment node can ssh access other nodes without the password. (the ssh user should has sudo permission)

other nodes(install docker):
sudo yum install -y https://yum.dockerproject.org/repo/main/centos/7/Packages/docker-engine-selinux-17.03.0.ce-1.el7.centos.noarch.rpm https://yum.dockerproject.org/repo/main/centos/7/Packages/docker-engine-17.03.0.ce-1.el7.centos.x86_64.rpm
```

6. Run command:

```
chmod +x ansible-k8s/tools/ansible-k8s

deploy:
ansible-k8s/tools/ansible-k8s deploy --configdir config-test/ -i config-test/multinode-inventory

deploy with tags:
ansible-k8s/tools/ansible-k8s deploy --configdir config-test/ -i config-test/multinode-inventory --tag kubelet

remove with tags:
ansible-k8s/tools/ansible-k8s remove --configdir config-test/ -i config-test/multinode-inventory --tag kubelet --yes-i-really-really-mean-it

remove all:

```

### Note

There are some checks on the configuration files and certificates when deploying. If there is a change, the corresponding service will be restarted, otherwise it will not be restarted.
So you can repeat the deployment to fix some problem programs.
