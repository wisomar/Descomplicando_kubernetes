### **DAY-1 – Fundamentos, Containers, OCI, Kubernetes e Primeiro Cluster**

---

## 🚀 Introdução

No **Day-1** do treinamento *Descomplicando o Kubernetes – Expert Mode*, damos os primeiros passos rumo ao mundo do Kubernetes.
Este dia é totalmente focado em te colocar no **mapa do Kubernetes**:

* O que é um container?
* Qual a diferença entre Container Engine e Container Runtime?
* O que é OCI?
* O que é o Kubernetes e como ele nasceu?
* Como funciona a arquitetura do Kubernetes?
* Como criar seu primeiro cluster local (Minikube, Kind, MicroK8s, etc.)
* Como executar seu **primeiro Pod real**
* Como expor o Pod criando um Service
* Como usar o kubectl e instalar o binário

👉 O objetivo do Day-1 é **construir sua base** para entender tudo o que virá nos próximos dias.

---

# 📌 O que você precisa saber antes de começar?

Durante o Day-1 você aprenderá:

* Conceitos fundamentais de containers
* Como funciona o Container Engine
* O que é o Container Runtime
* Como a **OCI (Open Container Initiative)** padronizou o ecossistema
* A história do Kubernetes
* Componentes do cluster: API Server, Scheduler, etcd, Kubelet, Kube-proxy
* Criar seu primeiro cluster Kubernetes
* Executar e expor seu primeiro Pod nginx

---

# 🐧 Qual distro GNU/Linux usar?

Ferramentas essenciais como **systemd** e **journald** já são padrão nas principais distribuições.

Recomendações:

* Ubuntu
* Debian
* CentOS / Rocky / AlmaLinux
* Fedora

👉 Qualquer uma dessas funcionará perfeitamente para acompanhar o curso.

---

# 🌍 Sites importantes para estudar

## 🌐 Documentação Kubernetes

* [https://kubernetes.io](https://kubernetes.io)
* [https://github.com/kubernetes/kubernetes/](https://github.com/kubernetes/kubernetes/)
* [https://github.com/kubernetes/kubernetes/issues](https://github.com/kubernetes/kubernetes/issues)

## 🎓 Certificações CNCF

* CKA — [https://www.cncf.io/certification/cka/](https://www.cncf.io/certification/cka/)
* CKAD — [https://www.cncf.io/certification/ckad/](https://www.cncf.io/certification/ckad/)
* CKS — [https://www.cncf.io/certification/cks/](https://www.cncf.io/certification/cks/)

---

# 🐳 Container Engine

O **Container Engine** é o componente responsável por:

* Gerenciar **imagens**
* Gerenciar **volumes**
* Garantir isolamento
* Controlar vida dos containers
* Integrar com o Kernel através do Runtime

Exemplos populares:

* **Docker**
* **CRIO**
* **Podman**

O Docker, por exemplo, utiliza o **containerd** como runtime.

Mas antes de entender runtimes, precisamos falar sobre a **OCI**.

---

# 🏛️ OCI – Open Container Initiative

A **OCI** padroniza:

* Formatos de imagens
* Formatos de containers
* Execução dos runtimes

Criada por:

* Docker
* CoreOS
* Google
* IBM
* Microsoft
* Red Hat
* VMware

O principal projeto da OCI é o **runc** — o runtime de baixo nível usado por engines como Docker.

---

# ⚙️ Container Runtime

O **Container Runtime** é o componente que realmente EXECUTA o container.
Ele fica abaixo do Container Engine.

Tipos de Runtime:

### 🔹 Low-level (executam direto no Kernel)

* runc
* crun
* gVisor runsc

### 🔹 High-level (gerenciadores intermediários)

* containerd
* CRI-O
* Podman

### 🔹 Sandboxed

* gVisor

### 🔹 Virtualized

* Kata Containers

---

# ☸️ O que é o Kubernetes?

## 📌 Versão resumida

O Kubernetes foi criado pelo Google em 2014 inspirado nos sistemas:

* Borg
* Omega

É um **orquestrador de containers** que gerencia aplicações distribuídas em larga escala.

“Kubernetes” = “Timoneiro” (grego)
K8s = abreviação (“k” + 8 letras + “s”)

---

## 📜 Versão longa (com contexto histórico)

* Google já rodava contêineres **há mais de 10 anos** antes do Kubernetes existir
* Borg → Omega → Kubernetes
* K8s focou em:

  * ser open source
  * escalável
  * fácil para desenvolvedores
  * baseado em arquitetura distribuída
  * abstrair infraestrutura

---

# 🧱 Arquitetura do Kubernetes

A arquitetura do Kubernetes é baseada em:

## 🧭 Control Plane

Componentes:

### **API Server**

Entrada oficial do cluster.
Toda ação passa por ele (kubectl, controllers, schedulers).

### **etcd**

Banco de dados chave-valor do cluster.
Armazena:

* estados
* configurações
* objetos

### **Scheduler**

Decide *em qual nó* cada Pod deve rodar.

### **Controller Manager**

Garante que o “estado atual” = “estado desejado”.

---

## 🖥️ Worker Nodes

### **Kubelet**

Agente que gerencia os containers em cada nó.

### **Kube-proxy**

Gerencia rede e regras de roteamento.

---

# 🔒 Portas importantes

## Control Plane

| Porta     | Protocolo | Componente         |
| --------- | --------- | ------------------ |
| 6443      | TCP       | API Server         |
| 2379–2380 | TCP       | etcd               |
| 10250     | TCP       | kubelet            |
| 10251     | TCP       | scheduler          |
| 10252     | TCP       | controller-manager |

## Worker Nodes

| Porta       | Protocolo | Componente        |
| ----------- | --------- | ----------------- |
| 10250       | TCP       | kubelet           |
| 30000–32767 | TCP       | NodePort Services |

---

# 🧩 Conceitos-chave do Kubernetes

* **Pod** → menor objeto do cluster
* **Deployment** → controla réplicas e atualizações
* **ReplicaSet** → garante quantidade de Pods
* **Service** → expõe Pods via ClusterIP / NodePort / LoadBalancer

---

# 🔧 Instalando o kubectl

## GNU/Linux

```bash
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version --client
```

## MacOS (Homebrew)

```bash
brew install kubectl
kubectl version --client
```

## Windows

Baixe em:
👉 [https://kubernetes.io/docs/tasks/tools/](https://kubernetes.io/docs/tasks/tools/)

---

# ⚙️ Customizando kubectl

## Autocomplete Bash

```bash
source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc
```

## Autocomplete ZSH

```bash
source <(kubectl completion zsh)
```

## Alias

```bash
alias k=kubectl
complete -F __start_kubectl k
```

---

# 🏗️ Criando seu primeiro cluster Kubernetes

## 🚀 Minikube (local cluster)

### Verificando suporte a virtualização

```bash
grep -E --color 'vmx|svm' /proc/cpuinfo
```

### Instalação Linux

```bash
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
chmod +x minikube
sudo mv minikube /usr/local/bin/
minikube version
```

### Iniciar cluster

```bash
minikube start
```

### Ver nodes

```bash
kubectl get nodes
```

---

## 🐳 Kind (Kubernetes in Docker)

Instalação Linux:

```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.14.0/kind-linux-amd64
chmod +x kind
sudo mv kind /usr/local/bin/kind
```

Criar cluster:

```bash
kind create cluster
```

Criar multinode:

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
  - role: worker
  - role: worker
```

---

# ▶️ Primeiro Pod no Kubernetes

Criar pod nginx:

```bash
kubectl run nginx --image=nginx
```

Ver Pods:

```bash
kubectl get pods
```

Remover:

```bash
kubectl delete pod nginx
```

---

# 📄 Criando Pod usando dry-run

Gerar template YAML:

```bash
kubectl run meu-nginx --image=nginx --dry-run=client -o yaml > pod-template.yaml
```

Criar Pod:

```bash
kubectl apply -f pod-template.yaml
```

---

# 🌐 Expondo o Pod — Criando um Service

Criar Service:

```bash
kubectl expose pod meu-nginx
```

Listar:

```bash
kubectl get svc
```

---

# 🧹 Limpando tudo

```bash
kubectl delete -f pod-template.yaml
kubectl delete service nginx
kubectl get all
```

---

# 🎓 Conclusão

No **Day-1**, você aprendeu:

✔ O que é um container, Engine e Runtime
✔ O papel da OCI
✔ A história e propósito do Kubernetes
✔ Componentes do Control Plane e Workers
✔ Como instalar e configurar o kubectl
✔ Como criar um cluster local (Minikube / Kind)
✔ Como criar seu primeiro Pod
✔ Como expor um Pod com um Service
✔ Como limpar tudo ao final

Este dia é **absolutamente fundamental** para tudo o que vem depois.

👉 No **Day-2**, você mergulhou nos Pods.
👉 No **Day-3**, estudou Deployments.
👉 Agora você está pronto para ingressar no **mundo real do Kubernetes**.

**#VAIIII 🚀🔥**


=======
# Descomplicando_kubernetes
>>>>>>> 0d01d9978809450b2b7d9796480de4e420ccd412
