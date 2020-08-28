# Kubernetes v1.18 高可用集群自动部署（在线版）

```
### 2、修改Ansible文件

修改hosts文件，根据规划修改对应IP和名称。

```
# vi hosts
...
```
修改group_vars/all.yml文件，修改软件包目录和证书可信任IP。

```
# vim group_vars/all.yml
software_dir: '/root/k8s_package'
...
cert_hosts:
  k8s:
  etcd:
```
## 3、一键部署
### 架构图
单Master架构
![avatar](https://github.com/lizhenliang/ansible-install-k8s/blob/master/single-master.jpg)

多Master架构
![avatar](https://github.com/lizhenliang/ansible-install-k8s/blob/master/multi-master.jpg)
### 部署命令
单Master版：
```
# ansible-playbook -i hosts single-master-deploy.yml -uroot -k
```
多Master版：
```
# ansible-playbook -i hosts multi-master-deploy.yml -uroot -k
```

## 4、部署控制
如果安装某个阶段失败，可针对性测试.

例如：只运行部署插件
```
# ansible-playbook -i hosts single-master-deploy.yml -uroot -k --tags addons
```

## 5、节点扩容
1）修改hosts，添加新节点ip
```
# vi hosts
```
2）执行部署
```
ansible-playbook -i hosts add-node.yml -uroot -k
```
3）在Master节点允许颁发证书并加入集群
```
kubectl get csr
kubectl certificate approve node-csr-xxx
```

