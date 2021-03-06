# Kubernetes Setup on Ubuntu

# For master and Node setup Manual Steps:

         $ sudo apt-get update
         $ sudo apt-get install -y apt-transport-https
         $ sudo su -
         $ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add
         $ cat <<EOF >/etc/apt/sources.list.d/kubernetes.list 
         deb https://apt.kubernetes.io/ kubernetes-xenial main 
         EOF
          $ sudo apt-get update
         $ sudo apt-get install -y docker.io
         $ sudo apt-get install -y kubelet kubeadm kubectl kubernetes-cni

# For Master only:

          $ sudo su - 
          $ kubeadm init
          $ mkdir -p $HOME/.kube
          $ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
          $ sudo chown $(id -u):$(id -g) $HOME/.kube/config


# For Nodes only;
Join token displayed while kubeadm init from the master 

          kubeadm join {master_ip}:6443 --token v2va6d.ktwswvnsadxa6addx3qvvkn --discovery-token-ca-cert-hash       sha357:76c9a8efb3757b57g72hc6d2d4c8ed1t4hy2bde091c550dd1f09f5a9b93165cb32eeee5

# Adding Flannel network updated link.
    flanneld needs a fix for k8s 1.12.
     Use this PR (till will be approved):
     kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml
     it's a known issue: coreos/flannel#1044

# K8 CMD's:

      kubectl get nodes
      kubectl get pods --all-namespace



       $ kubectl -n kube-system get secret clusterinfo -o yaml | grep token-map | awk '{print $2}' | base64 -d | sed "s|{||g;s|}||g;s|:|.|g;s/\"//g;" | xargs echo

      $ kubeadm join --discovery-token-unsafe-skip-ca-verification --token=`kubeadm token list` 193.19.0.923.2:6443

If the you lost the join token then you can re-print using below cmd

      $ sudo kubeadm token create --print-join-command  >>givesthe joining commands


 

      $ kubeadm join --discovery-token-unsafe-skip-ca-verification --token=`kubeadm token list` 193.19.0.923.2:6443
