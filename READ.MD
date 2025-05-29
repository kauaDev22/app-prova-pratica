
# Prova Prática - CI/CD com FastAPI, Docker, Kubernetes, Kustomize e ArgoCD

## 📦 Projeto

API simples desenvolvida com **FastAPI**, contendo 3 endpoints:

- `/` → retorna `{"message": "Hello World"}`
- `/square/{x}` → retorna o quadrado de `x`
- `/double/{x}` → retorna o dobro de `x`

---

## ⚙️ Tecnologias usadas

- FastAPI
- Pytest (⚠️ pendente de ajuste)
- Docker
- Docker Hub (`kaua010/prova-pratica`)
- GitHub Actions
- Kustomize
- Kubernetes (com Kind local)
- ArgoCD (deploy automatizado)

---

## 🧪 Testes (em andamento)

Os testes foram preparados com `pytest` e `TestClient` da FastAPI. O objetivo era testar os três endpoints da API. Os testes ainda precisam de ajustes nos imports devido à estrutura de pastas.

---

## 🐳 Docker

### Build da imagem:

```bash
docker build -t kaua010/prova-pratica:latest .
```

### Push para o Docker Hub:

```bash
docker push kaua010/prova-pratica:latest
```

---

## 🔁 CI/CD com GitHub Actions

### Workflow executa:

- Pytest (em construção)
- Criação da imagem Docker
- Push para Docker Hub
- Atualização do `kustomization.yaml` com nova tag
- Commit e push automático da nova tag

---

## ☸️ Kubernetes com Kustomize

Estrutura de diretórios:

```
k8s/
├── base/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── kustomization.yaml
└── overlays/
    └── dev/
        ├── kustomization.yaml
```

Comandos para aplicar:

```bash
kubectl apply -k k8s/overlays/dev
```

---

## 🔧 Cluster com Kind (local)

Criado com 3 nodes (1 control-plane, 2 workers):

```yaml
# kind-config.yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
```

Criação:

```bash
kind create cluster --config kind-config.yaml
```

---

## 🚀 ArgoCD

### Instalação

Instalado e configurado localmente para gerenciar o cluster Kind.

### Aplicação

Criado um `Application` no ArgoCD apontando para o repositório Git e pasta de configurações (`k8s/overlays/dev`).

- App Health: ✅ **Healthy**
- Sync Status: ⚠️ **Unknown**
- App Conditions: ❌ **1 Error**

> ⚠️ O erro ainda será investigado. Possíveis causas incluem imagem inválida ou recurso com falha de validação.

---

## ✅ Última etapa realizada

Na interface web do ArgoCD, a aplicação `prova-pratica` foi reconhecida e está **Healthy**, mas com um erro de condição e status de sincronização **Unknown**.

---

## 📌 Próximos passos

- Corrigir erro de import nos testes com pytest.
- Validar status de sincronização no ArgoCD.
- Testar o `port-forward` com `kubectl`:

```bash
kubectl port-forward svc/fastapi-service 8000:80
```

E acessar:

```
http://localhost:8000/
```

---

## 🧑‍💻 Autor

- Docker Hub: [kaua010](https://hub.docker.com/u/kaua010)

usei o chatgpt para organizar melhor os passos.