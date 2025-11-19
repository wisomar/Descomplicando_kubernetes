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