
How to install k8s on centos7


Step1:

$cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF

If network time out , can change the repo mirror for aliyun

$cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
exclude=kube*
EOF


And execute this: 

$yum-config-manager --disable kubernetes



Step2:


Install kubelet, kubeadm, and kubectl

$sudo yum install -y kubelet kubeadm kubectl
$systemctl enable kubelet
$systemctl start kubelet










vi /etc/hosts


XXX.16.3.145  master-node
XXX.16.3.146  work-node








Red Hat Subscription Manager订阅管理器，它会让你一直register，禁用就好。

$vim /etc/yum/pluginconf.d/subscription-manager.conf

[main]
enabled=0
再执行：
yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes

如果提示从缓存中取，Nothing to do , 清理缓存即可：

$yum clean all

systemctl enable kubelet
centos7用户还需要设置路由：

cat <<EOF >  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sysctl --system
Kubernetes 1.8开始要求关闭系统的Swap，如果不关闭，默认配置下kubelet将无法启动，关闭系统的Swap方法如下:

swapoff -a
修改 /etc/fstab 文件，注释掉 SWAP 的自动挂载，使用free -m确认swap已经关闭。
swappiness参数调整，修改/etc/sysctl.d/k8s.conf添加下面一行：

vm.swappiness=0
执行sysctl -p /etc/sysctl.d/k8s.conf使修改生效。




references:
1, https://phoenixnap.com/kb/how-to-install-kubernetes-on-centos
