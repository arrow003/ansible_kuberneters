## ansible install kubenetes

### 一.准备服务器并配置资源
| 主机名     | 角色     | OS  | 硬件 |IP|
| - | --| -| - | - |
| k8s-master01 | master | Alma Linux 8.6 | 2C4G |172.26.37.151|
| k8s-master02 | master | Alma Linux 8.6 | 2C4G |172.26.37.152|
| k8s-master03 | master | Alma Linux 8.6 | 2C4G |172.26.37.153|
|  | master-vip |  |  |172.26.37.156|
| k8s-node1    | node   | Alma Linux 8.6 | 2C4G |172.26.37.154|

1.给服务器做个snapshot

### 二.下载ansible安装的playbook等文件，并修改相关配置
2.下载ansible安装kubernetes集群的playbook等文件

```
$ git clone git@github.com:arrow003/ansible_kuberneters.git
```

3.配置./roles/base_init/files/hosts及/etc/ansible/hosts（待脚本自动化）

> ```
> # cat ./roles/base_init/files/hosts
> 127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
> ::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
> 172.26.37.151 k8s-master01
> 172.26.37.152 k8s-master02
> 172.26.37.153 k8s-master03
> 172.26.37.156 k8s-master-lb
> 172.26.37.154 k8s-node01
> 
> # cat /etc/ansible/hosts
> [k8s_master]
> 172.26.37.151  ansible_ssh_user=root ansible_ssh_pass='123456' 
> 172.26.37.152  ansible_ssh_user=root ansible_ssh_pass='123456'
> 172.26.37.153  ansible_ssh_user=root ansible_ssh_pass='123456'
> 
> [k8s_node]
> 172.26.37.154  ansible_ssh_user=root ansible_ssh_pass='123456'
> 
> 
> [k8s_all]
> 172.26.37.151  ansible_ssh_user=root ansible_ssh_pass='123456'
> 172.26.37.152  ansible_ssh_user=root ansible_ssh_pass='123456'
> 172.26.37.153  ansible_ssh_user=root ansible_ssh_pass='123456'
> 172.26.37.154  ansible_ssh_user=root ansible_ssh_pass='123456'
> 
> [k8s_etcd]
> 172.26.37.151  ansible_ssh_user=root ansible_ssh_pass='123456'
> 172.26.37.152  ansible_ssh_user=root ansible_ssh_pass='123456'
> 172.26.37.153  ansible_ssh_user=root ansible_ssh_pass='123456'
> 
> [k8s-master01]
> 172.26.37.151  ansible_ssh_user=root ansible_ssh_pass='123456'
> ```

4.修改参数

|  资源   | 版本|
| ----------- | -------------- |
| 系统Version    | Alma Linux 8.6|
| DockerVersion  | 20.10.x|
| Pod网段     | 172.36.0.0/16  |
| Service网段 | 192.168.0.0/16 |
| K8sVersion | 1.23.0 |
| etcdVersion | 3.5.1|

### 三.执行配置服务器准备及安装kubernetes集群操作
5.配置kubernetes集群服务器

 ```
$ ansible-playbook base_server.yml 
 ```
6.安装kubernetes集群

```
$ ansible-playbook install_k8s_cluster.yml 
```

### 四.验证kubernetes集群

7.验证kubernetes集群

```
$ ansible-playbook check_k8s_cluster.yml
```
