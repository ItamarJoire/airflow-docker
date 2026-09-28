# Executando o Apache Airflow com Docker

Este guia fornece as instruções necessárias para configurar e executar um ambiente de desenvolvimento do **Apache Airflow** localmente utilizando **Docker Compose**.

---

## Pré-requisitos

* **Docker Desktop** instalado e em execução.
* Terminal de comando (Bash, PowerShell ou CMD).

---

## Passo a Passo de Instalação

### 1. Estruturação do Projeto

Crie o diretório do projeto e as pastas essenciais que serão mapeadas como volumes nos containers do Airflow:

```bash
# Criar diretório do projeto
mkdir airflow-docker
cd airflow-docker

# Criar subdiretórios essenciais
mkdir -p dags config logs plugins
```

### 2. Obter o `docker-compose.yaml`

Faça o download do arquivo de configuração oficial do Apache Airflow e salve-o na raiz da pasta do projeto:

* [Download do docker-compose.yaml oficial](https://airflow.apache.org/docs/apache-airflow/stable/howto/docker-compose/index.html#fetching-docker-compose-yaml)

*Ou via terminal:*

```bash
# Linux/macOS / PowerShell
curl -LfO 'https://airflow.apache.org/docs/apache-airflow/stable/docker-compose.yaml'
```

---

### 3. Configuração do Ambiente (`.env`)

Crie o arquivo `.env` na raiz do projeto e defina o ID do usuário para evitar problemas de permissão nos volumes montados:

```bash
echo "AIRFLOW_UID=50000" > .env
```

> **Nota:** Se necessário, você também pode adicionar a chave de criptografia (`FERNET_KEY`) no arquivo `.env`.

---

### 4. Inicialização do Ambiente

Antes de subir os serviços pela primeira vez, execute o serviço de inicialização para preparar o banco de dados e aplicar as migrações:

```bash
docker compose up airflow-init
```

Após a conclusão com sucesso da inicialização (`airflow-init` finalizado com código 0), inicie todos os serviços em segundo plano:

```bash
docker compose up -d
```

---

## Entendendo os Comandos Docker

* **`docker compose up`**: Lê o arquivo `docker-compose.yaml`, baixa as imagens necessárias, cria as redes, volumes e inicia os containers.
* **`-d` (`--detach`)**: Executa os containers em **segundo plano** (modo *detached*).
  * **Com `-d`**: Os containers rodam silenciosamente e o terminal fica livre para uso imediato.
  * **Sem `-d`**: Os logs de todos os containers são exibidos diretamente no terminal, prendendo a sessão atual.

---

## Acesso à Interface Web

Após a execução dos containers, acesse o painel interativo do Airflow:

* **URL:** [http://localhost:8080/](http://localhost:8080/)
* **Usuário padrão:** `airflow`
* **Senha padrão:** `airflow`

---

## Comandos Úteis de Manutenção

* **Verificar status dos serviços:**
  ```bash
  docker compose ps
  ```

* **Acompanhar os logs em tempo real:**
  ```bash
  docker compose logs -f
  ```

* **Parar e remover os containers:**
  ```bash
  docker compose down
  ```