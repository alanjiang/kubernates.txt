
How to install k8s on centos7


Step1:

cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF

Step2:


Install kubelet, kubeadm, and kubectl


sudo yum install -y kubelet kubeadm kubectl
systemctl enable kubelet
systemctl start kubelet










vi /etc/hosts


XXX.16.3.145  master-node
XXX.16.3.146  work-node

































references:
1, https://phoenixnap.com/kb/how-to-install-kubernetes-on-centos
