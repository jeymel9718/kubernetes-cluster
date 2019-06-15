# Proyecto2
## Prerequisitos
- [Vagrant](https://www.vagrantup.com/downloads.html)
- Virtual Box
- Kubectl
- Kubeadm

## Inicializar VMs
Correr `$ vagrant up` para inicializar las maquinas virtuales.
_Este comando debe ser ejecutado dentro de un directorio con un **Vagrantfile**_

## Kubernetes
### Inicializar kubernetes
- ssh dentro del nodo manager `$ vagrant ssh manager`
- `$ sudo su`
- `$ swapoff -a`
- `$ systemctl restart kubelet.service`
- `kubeadm reset`
- Realizar los comandos anteriores dentro de los nodos
- Los siguientes comandos se deben ejecutar solamente en el master
- `$ sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address=10.133.7.100 --apiserver-cert-extra-sans=10.0.2.15`
- `mkdir -p $HOME/.kube`
- `sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config`
- `sudo chown $(id -u):$(id -g) $HOME/.kube/config`
### Instalar pod Network
Kubernetes necesita de una pod network para poder funcionar, la lista de pod networks disponibles se puede ver aqui en el [tutorial de kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/#master-isolation), la network selecciona para esta configuración es Canal
- `kubectl apply -f https://docs.projectcalico.org/v3.7/manifests/canal.yaml`
- `kubectl taint nodes --all node-role.kubernetes.io/master-`
### Añadiendo nodos
El comando para añadir nodos es producido por la salida del comando kubeadm init
`kubeadm join 10.133.7.100:6443 --token abcdefghijklmnopqrstuvw --discovery-token-ca-cert-hash sha256:abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijkl`

## Inicializar los servicios
- `$ cd services`
Dentro de la carpeta services se encuentra un archivo para iniciar cada servicio y pod, antes se debe crear el servicio de metallb para la asignacion de ip externas
- `$ kubectl apply -f https://raw.githubusercontent.com/google/metallb/v0.7.3/manifests/metallb.yaml`
- `$ kubectl apply -f metallb-config.yaml`
- `$ kubectl apply -f go-deploy.yaml`
- `$ kubectl apply -f mysql-pv.yaml`
- `$ kubectl apply -f mysql-deployment.yaml`
- `$ kubectl apply -f ruby-deploy.yaml`
- `$ kubectl apply -f node-deploy.yaml`
- `$ kubectl apply -f gateway.yaml`
