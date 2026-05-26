# RACK+

**Status do Projeto:** Em desenvolvimento 🚧

Sistema web de monitoramento e gerenciamento de dispositivos desenvolvido como projeto de TCC.

---

## Sobre o Projeto

Plataforma desenvolvida para ambientes técnicos e corporativos com o objetivo de centralizar a gestão de infraestrutura de TI. O sistema permite o monitoramento de dispositivos organizados hierarquicamente em **Salas → Racks → Devices**, facilitando a identificação rápida de falhas e o gerenciamento do inventário técnico através de uma interface web responsiva e uma API REST para integração com agentes externos de coleta.

---

## Objetivo

Desenvolver uma aplicação web moderna e responsiva capaz de:

- Monitorar dispositivos em tempo real via agente externo de coleta
- Exibir status dos equipamentos com indicadores visuais (verde/amarelo/vermelho)
- Centralizar o inventário de infraestrutura em um dashboard hierárquico
- Registrar e consultar histórico de telemetria por dispositivo
- Facilitar o gerenciamento técnico com CRUD completo via interface web e API REST

---

## 🛠️ Tecnologias Utilizadas

| Camada | Tecnologias |
| :--- | :--- |
| **Backend** | Python 3.12, Django 6, Django REST Framework |
| **Autenticação** | JWT via djangorestframework-simplejwt, PyJWT |
| **Frontend** | HTML5, CSS3, JavaScript, Bootstrap 5 |
| **Banco de Dados** | SQLite (desenvolvimento local), MySQL (produção) |
| **Dependências** | python-decouple, mysqlclient |
| **Ferramentas** | Git, GitHub, VS Code |

---

## ⚙️ Instalação e Configuração

O projeto usa uma arquitetura de settings separados:

- `settings_base.py` — configurações compartilhadas entre todos os ambientes
- `settings_local.py` — configuração de desenvolvimento com SQLite, já incluso no repositório
- `settings.py` — configuração de produção com MySQL, requer `.env`

O `manage.py` aponta para `settings_local` por padrão, portanto **nenhuma configuração adicional é necessária para rodar localmente**.

### Pré-requisitos

- Python 3.12 ou superior
- Git

---

### 1. Clone o repositório

```bash
git clone https://github.com/turma-ipi-rubens/repo-rack-plus.git
cd repo-rack-plus
```

---

### 2. Crie e ative o ambiente virtual

**Windows:**
```bash
python -m venv venv
venv\Scripts\activate
```

**Linux / macOS:**
```bash
python3 -m venv venv
source venv/bin/activate
```

---

### 3. Instale as dependências

```bash
pip install -r requirements.txt
```

---

### 4. Aplique as migrações

```bash
python manage.py migrate
```

---

### 5. Crie o superusuário

```bash
python manage.py createsuperuser
```

Siga as instruções do terminal para definir usuário, e-mail (opcional) e senha.

---

### 6. Inicie o servidor

```bash
python manage.py runserver
```

A aplicação estará disponível em:

- Interface web: [http://127.0.0.1:8000](http://127.0.0.1:8000)
- Painel admin: [http://127.0.0.1:8000/admin](http://127.0.0.1:8000/admin)

---

## 🐬 Configuração para Produção (MySQL) - OPCIONAL

Para rodar em produção com MySQL, crie o banco, configure o `.env` e troque a linha do `manage.py`.

### 1. Crie o banco de dados

```sql
CREATE DATABASE rackplus CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### 2. Crie o arquivo `.env` na raiz do projeto

```env
SECRET_KEY=cole_aqui_uma_chave_secreta
DEBUG=False
DB_NAME=rackplus
DB_USER=root
DB_PASSWORD=sua_senha_do_mysql
DB_HOST=localhost
DB_PORT=3306
```

> Gere uma `SECRET_KEY` segura em: **https://djecrety.ir/**

### 3. Aponte o `manage.py` para o settings de produção

No arquivo `manage.py`, troque:

```python
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'core.settings_local')
```

por:

```python
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'core.settings')
```

### 4. Aplique as migrações

```bash
python manage.py migrate
```

> **Nota técnica — driver de conexão:** no **Linux (Ubuntu/Debian)**, caso ocorra erro de compilação durante o `pip install`, instale as dependências do sistema antes:
> ```bash
> sudo apt install python3-dev default-libmysqlclient-dev build-essential
> ```

---

## 🔐 Autenticação JWT (API)

A API utiliza autenticação via JWT. Para consumir qualquer endpoint é necessário obter um token primeiro.

### Obter token

```
POST /api/token/
```

```json
{
  "username": "seu_usuario",
  "password": "sua_senha"
}
```

Retorna:

```json
{
  "access": "eyJ...",
  "refresh": "eyJ..."
}
```

### Usar o token nas requisições

```
Authorization: Bearer eyJ...
```

### Renovar o token

```
POST /api/token/refresh/
```

```json
{
  "refresh": "eyJ..."
}
```

> O access token expira em **30 minutos**. O refresh token expira em **7 dias** e é rotacionado a cada uso — o token antigo é automaticamente invalidado via blacklist.

---

## 🌐 API REST

### Endpoints disponíveis

| Método | Rota | Descrição |
| :--- | :--- | :--- |
| `POST` | `/api/token/` | Obter access e refresh token |
| `POST` | `/api/token/refresh/` | Renovar access token |
| `GET` | `/api/salas/` | Listar todas as salas |
| `POST` | `/api/salas/` | Criar uma sala |
| `GET` | `/api/salas/<id>/` | Detalhe de uma sala |
| `PUT` | `/api/salas/<id>/` | Atualizar uma sala |
| `DELETE` | `/api/salas/<id>/` | Remover uma sala |

> Novos endpoints serão adicionados conforme o desenvolvimento avança.

---

## Cronograma

| Etapa | Previsão |
| :--- | :--- |
| Lançamento do MVP | 15/06/2026 |
| Lançamento v1.0 | 01/07/2026 |

---

## Equipe Responsável

| Membro | GitHub |
| :--- | :--- |
| **Caio Gomes** (Representante) | [@CaioGomes26](https://github.com/CaioGomes26) |
| Daniel Campos | [@leinad52](https://github.com/leinad52) |
| Jenifer Ventura | [@Refinej52](https://github.com/Refinej52) |
| Rafael Eloisio | [@0f4el](https://github.com/0f4el) |
| Tiago Martins | [@Tiagonaoeum-dev](https://github.com/Tiagonaoeum-dev) |

---

## Licença

Este projeto possui finalidade estritamente acadêmica e educacional.
