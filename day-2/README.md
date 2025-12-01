
# Descomplicando o Kubernetes - Expert Mode

## **DAY-2 - Trabalhando com Pods**

---

## 📚 **Índice**

1. [O que iremos ver hoje?](#o-que-iremos-ver-hoje)
2. [O que é um Pod?](#o-que-é-um-pod)
3. [Criando um Pod](#criando-um-pod)
4. [Criando um Pod via YAML](#criando-um-pod-através-de-um-arquivo-yaml)
5. [Pod com múltiplos containers](#criando-um-pod-com-mais-de-um-container)
6. [Containers com limites de CPU e memória](#criando-um-container-com-limites-de-memória-e-cpu)
7. [Testando limites com stress](#testando-os-limites)
8. [Volume EmptyDir](#adicionando-um-volume-emptydir-no-pod)

---

## 🧠 **O que iremos ver hoje?**

Hoje vamos nos aprofundar no menor objeto do Kubernetes: o **Pod**.

Você aprenderá:
✔ O que é um Pod
✔ Criar Pods por comando e por YAML
✔ Pods com múltiplos containers
✔ Definir limites de CPU e memória
✔ Utilizar volumes do tipo EmptyDir
✔ Ver detalhes, logs, descrever Pods e interagir com containers

---

## 📦 **O que é um Pod?**

O **Pod é a menor unidade do Kubernetes**.

Ele funciona como uma "caixinha" que contém **um ou mais containers**, que compartilham:

* IP
* Namespaces
* Volumes
* Recursos

Sempre pense: **um Pod = um ou mais containers que compartilham recursos**.

---

## 🚀 **Criando um Pod**

Existem duas formas principais:

### ➤ **1. Via comando**

```bash
kubectl run giropops --image=nginx --port=80
```

### Ver Pods:

```bash
kubectl get pods
kubectl get pods -A
kubectl get pods -n <namespace>
```

### Mais detalhes:

```bash
kubectl get pod giropops -o yaml
kubectl get pod giropops -o json
kubectl get pod giropops -o wide
kubectl describe pod giropops
```

### Remover:

```bash
kubectl delete pod giropops
```

---

## 📄 **Criando um Pod através de um arquivo YAML**

Arquivo: `pod.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: giropops
  labels:
    run: giropops
spec:
  containers:
  - name: giropops
    image: nginx
    ports:
    - containerPort: 80
```

Criar o Pod:

```bash
kubectl apply -f pod.yaml
```

Ver logs:

```bash
kubectl logs giropops
kubectl logs -f giropops
```

Excluir:

```bash
kubectl delete pod giropops
```

---

## 🧩 **Criando um Pod com mais de um container**

Arquivo: `pod-multi-container.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: giropops
  labels:
    run: giropops
spec:
  containers:
  - name: girus
    image: nginx
    ports:
    - containerPort: 80
  - name: strigus
    image: alpine
    args:
    - sleep
    - "1800"
```

Criar:

```bash
kubectl apply -f pod-multi-container.yaml
```

### Interagir com containers

**attach (sem iniciar processo novo):**

```bash
kubectl attach giropops -c strigus
```

**exec (criando um processo dentro do container):**

```bash
kubectl exec giropops -c strigus -- ls
kubectl exec giropops -c strigus -it -- sh
kubectl exec giropops -c girus -it -- sh
```

---

## 🧮 **Criando um container com limites de memória e CPU**

Arquivo: `pod-limitado.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: giropops
  labels:
    run: giropops
spec:
  containers:
  - name: girus
    image: nginx
    ports:
    - containerPort: 80
    resources:
      limits:
        memory: "128Mi"
        cpu: "0.5"
      requests:
        memory: "64Mi"
        cpu: "0.3"
```

Criar:

```bash
kubectl create -f pod-limitado.yaml
```

Ver detalhes:

```bash
kubectl describe pod giropops
```

---

## 🧪 **Testando os limites**

Criar Pod Ubuntu para testes:

Arquivo: `pod-ubuntu-limitado.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: giropops
spec:
  containers:
  - name: girus
    image: ubuntu
    args:
    - sleep
    - infinity
    resources:
      limits:
        memory: "128Mi"
        cpu: "0.5"
      requests:
        memory: "64Mi"
        cpu: "0.3"
```

Criar:

```bash
kubectl create -f pod-ubuntu-limitado.yaml
```

Entrar no container:

```bash
kubectl exec -it giropops -- bash
```

Instalar stress:

```bash
apt update
apt install -y stress
```

Testes:

```bash
stress --vm 1 --vm-bytes 100M
stress --vm 1 --vm-bytes 200M   # deve falhar
```

---

## 📁 **Adicionando um volume EmptyDir no Pod**

Arquivo: `pod-emptydir.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: giropops
spec:
  containers:
  - name: girus
    image: ubuntu
    args:
    - sleep
    - infinity
    volumeMounts:
    - name: primeiro-emptydir
      mountPath: /giropops
  volumes:
  - name: primeiro-emptydir
    emptyDir:
      SizeLimit: 256Mi
```

Criar:

```bash
kubectl create -f pod-emptydir.yaml
```

Entrar no container:

```bash
kubectl exec -it giropops -- bash
```

Criar arquivo:

```bash
touch /giropops/FUNCIONAAAAAA
```

Ver montagens:

```bash
mount
```

---

## 🎉 **Conclusão**

Agora você domina tudo relacionado a Pods:
✔ Criação
✔ Logs
✔ Describe
✔ Exec e attach
✔ Múltiplos containers
✔ Limites de CPU/memória
✔ Volumes EmptyDir

Essa base é **fundamental** para seguir no restante da trilha Kubernetes Expert Mode.

---

### 🔗 Documentações recomendadas

* Kubernetes Pods: [https://kubernetes.io/docs/concepts/workloads/pods/](https://kubernetes.io/docs/concepts/workloads/pods/)
* YAML Kubernetes: [https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/)
* Kubectl Reference: [https://kubernetes.io/docs/reference/kubectl/](https://kubernetes.io/docs/reference/kubectl/)
* Recursos e Limits: [https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)



# Comandos Básicos Kubernetes

Este documento contém uma coleção de comandos essenciais do `kubectl` para gerenciamento e troubleshooting de clusters Kubernetes.

## 📋 Índice

- [Cluster Autoscaler](#cluster-autoscaler)
- [Monitoramento](#monitoramento)
- [Ingress NGINX](#ingress-nginx)
- [Gerenciamento de Pods](#gerenciamento-de-pods)
- [Execução de Comandos](#execução-de-comandos)
- [Multi Containers](#multi-containers)

---

## 🔧 Cluster Autoscaler

### Editar configuração do autoscaler
```bash
kubectl -n kube-system edit deploy cluster-autoscaler
```
Permite editar as configurações do Cluster Autoscaler diretamente no cluster.

### Visualizar logs do autoscaler
```bash
kubectl -n kube-system logs deploy/cluster-autoscaler -c cluster-autoscaler --since=5m
```
Exibe os logs dos últimos 5 minutos do Cluster Autoscaler para debug e monitoramento.

---

## 📊 Monitoramento

### Verificar HPA em tempo real
```bash
kubectl get hpa --watch
```
Monitora o Horizontal Pod Autoscaler em tempo real, mostrando mudanças conforme ocorrem.

### Listar todos os pods do cluster
```bash
kubectl get pods -A
```
Mostra todos os pods de todos os namespaces do cluster Kubernetes.

### Listar pods de um namespace específico
```bash
kubectl get pods -n <NAMESPACE>
```
Lista todos os pods dentro de um namespace específico do cluster.

---

## 🌐 Ingress NGINX

### Verificar pods do NGINX
```bash
kubectl get pods -n ingress-nginx -o wide
```
Lista os pods do Ingress NGINX com informações detalhadas como IP e nó.

### Verificar serviços do NGINX
```bash
kubectl get svc -n ingress-nginx
```
Exibe os serviços configurados no namespace do Ingress NGINX.

---

## 🎯 Gerenciamento de Pods

### Descrever detalhes de um pod
```bash
kubectl describe pod <NOME_DO_POD>
```
Mostra todos os detalhes internos do pod — eventos, contêineres, status e possíveis erros.

### Exibir pods em formato YAML
```bash
kubectl get pods -o yaml
```
Exibe a definição completa dos pods em formato YAML, exatamente como o Kubernetes armazena internamente.

### Exibir pods com informações extras
```bash
kubectl get pods -o wide
```
Mostra os pods com informações extras, como IP, nó onde estão rodando, imagens e outras colunas adicionais.

### Exibir pods com labels específicos
```bash
kubectl get pods -L <NOME_DO_LABEL>
```
Adiciona uma coluna na listagem mostrando o valor daquele label em cada pod.

### Deletar um pod
```bash
kubectl delete pod <NOME_DO_POD>
```
Exclui um pod do cluster, fazendo o Kubernetes recriá-lo (se estiver controlado por um Deployment/ReplicaSet).

### Criar um pod rapidamente
```bash
kubectl run <NOME_DO_POD> --image=<IMAGEM> --port 80
```
**Exemplo:**
```bash
kubectl run will-imagem --image nginx --port 80
```
Cria rapidamente um pod a partir de uma imagem de contêiner, ideal para testes.

---

## 💻 Execução de Comandos

### Abrir shell interativo no pod
```bash
kubectl exec -it <NOME_DO_POD> -- sh
```
Abre um shell interativo dentro do pod, permitindo executar comandos dentro dele.

### Executar comando específico em um container
```bash
kubectl exec <NOME_DO_POD> -c <NOME_DO_CONTAINER> -- cat <CAMINHO_DO_ARQUIVO>
```
**Exemplo:**
```bash
kubectl exec meu-pod -c nginx -- cat /usr/share/nginx/html/index.html
```
Executa o comando `cat` dentro do pod para mostrar o conteúdo do arquivo especificado.

---

## 🔀 Multi Containers

### Gerar YAML sem criar o pod
```bash
kubectl run <NOME_DO_POD> --image=alpine --dry-run=client -o yaml
```
Gera o YAML de um pod Alpine sem criá-lo, apenas simulando a criação. Útil para criar templates.

### Visualizar logs de um container específico
```bash
kubectl logs <NOME_DO_POD> -c <NOME_DO_CONTAINER>
```
Permite visualizar os logs de um contêiner específico dentro de um pod com múltiplos containers.

---

## 📝 Dicas Úteis

- Use `--help` em qualquer comando para ver todas as opções disponíveis
- Combine comandos com `grep` para filtrar resultados específicos
- Use `kubectl explain <recurso>` para entender a estrutura de qualquer recurso Kubernetes
- Configure aliases para comandos frequentes no seu `.bashrc` ou `.zshrc`:
  ```bash
  alias k='kubectl'
  alias kgp='kubectl get pods'
  alias kgpa='kubectl get pods -A'
  ```

---

## 📚 Recursos Adicionais

- [Documentação Oficial Kubernetes](https://kubernetes.io/docs/)
- [Kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
- [Kubernetes API Reference](https://kubernetes.io/docs/reference/kubernetes-api/)

---

**Nota:** Substitua `<NOME_DO_POD>`, `<NAMESPACE>`, `<NOME_DO_CONTAINER>` e outros placeholders pelos valores reais do seu ambiente.


