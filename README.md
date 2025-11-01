## Projeto de Demonstração DevOps: CI/CD e Gerenciamento de Artefatos

Este repositório demonstra a implementação de um fluxo DevOps completo.

**Nota Importante (Justificativa Técnica):**
Originalmente, a arquitetura foi planejada para ser **Open Source e Auto-Hospedada** (usando GitLab CE e seu Registry local). No entanto, durante a fase de implementação, encontramos dificuldades críticas com a autenticação no Registry local, especificamente:

> **Falha na etapa de `docker login` devido a problemas de Certificados (Self-Signed Certificates) e complexidade de configuração de rede (Docker-in-Docker) entre o Runner e o Registry auto-hospedado.**

Para garantir a execução funcional do pipeline e a entrega no prazo, migramos a arquitetura para serviços **SaaS (Software as a Service)**, que resolvem a cadeia de confiança e a infraestrutura de rede.

---

## 1. Arquitetura e Componentes

O fluxo agora é construído em torno da integração do GitHub e GitHub Actions, com o **Docker Hub** como registro final de artefatos.

| Componente | Ferramenta Utilizada | Detalhe Técnico |
| :--- | :--- | :--- |
| **Versionamento & Colaboração** | **GitHub** | Repositório Git e uso de Pull Requests (PRs). |
| **Integração Contínua (CI)** | **GitHub Actions** | Ferramenta SaaS para automação do pipeline, configurada via arquivo `.github/workflows/ci-cd-pipeline.yml`. |
| **Aplicação** | **Python 3.11** | Build da aplicação a partir das dependências em `requirements.txt`. |
| **Registro de Artefatos** | **Docker Hub** | Serviço de nuvem para o armazenamento da imagem Docker final. |

---

## 2. Fluxo de Integração Contínua Automatizado

O pipeline `CI/CD Pipeline` é executado pelo GitHub Actions e garante que, ao fazer o push do código, o artefato seja construído e publicado.

### Disparo e Automação

O pipeline é **acionado automaticamente** (Trigger) sob duas condições:
* Qualquer `push` na branch principal (`main`).
* Abertura ou atualização de um `pull_request` para a branch principal (`main`).

### Etapas Chave do Pipeline

O workflow de CI/CD automatiza as seguintes ações:

| Ação Executada | Descrição Detalhada |
| :--- | :--- |
| **Configurar Ambiente** | Baixa o código e configura o ambiente Python 3.11, instalando as dependências via `pip install -r requirements.txt`. |
| **Build da Imagem Docker** | Geração da imagem Docker, utilizando o `Dockerfile` na raiz do projeto e taggeando-a como `v1.0.0`. |
| **Login no Docker Hub** | Autentica no Docker Hub utilizando as *secrets* do repositório. |
| **Publicação do Artefato** | Envia a imagem taggeada para o repositório do usuário no Docker Hub. |

---

## 3. Arquivos Incluídos

Este repositório contém:
* O código-fonte da aplicação Python.
* O `Dockerfile`.
* O arquivo de configuração do pipeline (`.github/workflows/*.yml`).

A demonstração prática será focada na visualização da execução bem-sucedida do pipeline no GitHub Actions e na presença da imagem `v1.0.0` no Docker Hub.