# 📘 README.md — *Descomplicando o Kubernetes – Expert Mode*

### **DAY-3 – Deployment**

---

## 🚀 Introdução

No **Day-3** do treinamento *Descomplicando o Kubernetes – Expert Mode*, estudamos um dos componentes **mais importantes** para executar aplicações no Kubernetes: o **Deployment**.

Agora que já entendemos Pods (Day-2), vamos dar um passo além e aprender como o Kubernetes gerencia aplicações reais em produção.

---

# 📌 O que é um Deployment?

Um **Deployment** é um objeto Kubernetes responsável por:

* Descrever **como a aplicação deve rodar**
* Garantir que o número desejado de Pods esteja sempre ativo
* Controlar a criação, remoção e atualização dos Pods
* Gerenciar automaticamente **ReplicaSets**
* Permitir **atualizações** e **rollback** de forma segura
* Aplicar estratégias de atualização como **RollingUpdate** e **Recreate**

O Deployment funciona como um **controlador declarativo**:
👉 você define o “estado desejado” e o Kubernetes faz o resto.

---

# 🧱 Como criar um Deployment?

Crie um arquivo chamado **deployment.yaml**:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: nginx-deployment
  name: nginx-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-deployment
  strategy: {}
  template:
    metadata:
      labels:
        app: nginx-deployment
    spec:
      containers:
      - image: nginx
        name: nginx
        resources:
          limits:
            cpu: "0.5"
            memory: 256Mi
          requests:
            cpu: 0.25
            memory: 128Mi
```

---

# 📌 Explicando cada parte do manifesto

### **apiVersion / kind**

Define o tipo do objeto que estamos criando.

```yaml
apiVersion: apps/v1  
kind: Deployment
```

---

### **metadata**

Define nome e labels do Deployment.

```yaml
metadata:
  labels:
    app: nginx-deployment
  name: nginx-deployment
```

---

### **spec.replicas**

Quantidade de réplicas desejadas.

```yaml
spec:
  replicas: 3
```

---

### **selector**

Define como o Deployment identifica os Pods que ele vai gerenciar.

```yaml
selector:
  matchLabels:
    app: nginx-deployment
```

---

### **template**

Modelo usado para gerar os Pods gerenciados.

```yaml
template:
  metadata:
    labels:
      app: nginx-deployment
  spec:
    containers:
    - image: nginx
      name: nginx
```

---

# 🚀 Criando o Deployment

```bash
kubectl apply -f deployment.yaml
```

---

# 🔍 Consultando recursos do Deployment

### Ver Deployments

```bash
kubectl get deployments -l app=nginx-deployment
```

### Ver Pods gerenciados

```bash
kubectl get pods -l app=nginx-deployment
```

### Ver ReplicaSets gerenciados

```bash
kubectl get replicasets -l app=nginx-deployment
```

### Descrever detalhes do Deployment

```bash
kubectl describe deployment nginx-deployment
```

---

# 🔄 Atualizando Deployments

Alterar a imagem no manifesto:

```yaml
image: nginx:1.16.0
```

Aplicar novamente:

```bash
kubectl apply -f deployment.yaml
```

Verificar:

```bash
kubectl describe deployment nginx-deployment
```

---

# 🎯 Estratégias de atualização (Update Strategies)

O Kubernetes suporta duas estratégias principais:

## 1️⃣ RollingUpdate (padrão)

Atualiza gradualmente os Pods, garantindo disponibilidade.

Exemplo:

```yaml
strategy:
  type: RollingUpdate
  rollingUpdate:
    maxSurge: 1
    maxUnavailable: 2
```

Significa:

* **maxSurge**: pode criar 1 Pod extra durante a atualização
* **maxUnavailable**: até 2 Pods podem ficar indisponíveis
* Atualização gradual e segura

Acompanhar rollout:

```bash
kubectl rollout status deployment nginx-deployment
```

---

## 2️⃣ Recreate

* Destroi todos os Pods
* Cria novos com a nova versão
* Simples e rápido
* Pode gerar **indisponibilidade**

Exemplo:

```yaml
strategy:
  type: Recreate
```

---

# 🔙 Rollback (Desfazer atualização)

### Voltar para versão anterior

```bash
kubectl rollout undo deployment nginx-deployment
```

### Ver histórico

```bash
kubectl rollout history deployment nginx-deployment
```

### Ver detalhes de uma revisão

```bash
kubectl rollout history deployment nginx-deployment --revision=1
```

### Rollback para revisão específica

```bash
kubectl rollout undo deployment nginx-deployment --to-revision=1
```

---

# 🧰 Comandos úteis do `kubectl rollout`

| Comando                                | Função                |
| -------------------------------------- | --------------------- |
| `kubectl rollout status deployment X`  | Acompanha o rollout   |
| `kubectl rollout history deployment X` | Mostra o histórico    |
| `kubectl rollout undo deployment X`    | Volta versão          |
| `kubectl rollout pause deployment X`   | Pausa atualizações    |
| `kubectl rollout resume deployment X`  | Retoma atualizações   |
| `kubectl rollout restart deployment X` | Reinicia o Deployment |

---

# ❌ Removendo um Deployment

### Pelo nome:

```bash
kubectl delete deployment nginx-deployment
```

### Pelo manifesto:

```bash
kubectl delete -f deployment.yaml
```

---

# 🎓 Conclusão

No **Day-3**, aprendemos:

* O que é um Deployment
* Como ele funciona
* Como criar, atualizar e remover
* Como funcionam RollingUpdate e Recreate
* Como fazer rollback
* Como visualizar histórico de versões
* Como acompanhar rollouts em tempo real

Agora você tem uma base sólida para trabalhar com Deployments no Kubernetes.

👉 No **Day-4**, veremos **ReplicaSets** e **DaemonSets**!
**#VAIIII 🚀🔥**
