<p align="center">
<img src="https://cwmkt.com.br/wp-content/uploads/2023/08/logo-github-cwmkt.svg" alt="DispZap Whats Marketing" width="240" />
<p align="center">Seja bem-vindo ao Guia de atualização do n8n, nodejs e quepasa 🚀</p>
</p>
  
<p align="center">
<img src="https://whatsapp.com/favicon.ico" alt="WhatsAPP-logo" width="32" />
<span>Grupo WhatsaAPP: </span>
<a href="https://link.cwmkt.com.br/grupo-whats" target="_blank">Grupo</a>
</p>

<hr />
<hr />

<details>
<summary>Manual de Instalação N8N</summary>

#### Para seu Chatwoot funcionar corretamente com API Quepasa, instale a versão abaixo compativél

Migração de banco de dados sqlite para Postgres

### Criando Banco de dados Usuario e Senha

```bash
sudo -i -u postgres psql
```

```bash
CREATE ROLE n8n_user WITH LOGIN PASSWORD 'SenhaAqui';
```

```bash
CREATE DATABASE n8n_db;
```

```bash
GRANT ALL PRIVILEGES ON DATABASE n8n_db TO n8n_user;
```

```bash
GRANT CONNECT ON DATABASE n8n_db TO n8n_user;
```

```bash
\q
```

### Remova Node.js instalado pelo Chatwoot

```bash
sudo apt-get remove nodejs
```

```bash
sudo apt-get purge nodejs
```

```bash
sudo apt-get autoremove
```

### Instale a versão v18.x
Baixe e importe a chave Nodesource GPG

```bash
sudo apt-get update
```

```bash
sudo apt-get install -y ca-certificates curl gnupg
```

```bash
sudo mkdir -p /etc/apt/keyrings
```

```bash
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
```

Criar repositório deb

```bash
NODE_MAJOR=18
```

```bash
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list
```

Execute a atualização e instale

```bash
sudo apt-get update
```

```bash
sudo apt-get install nodejs -y
```
### Instale a última versão do n8n

> A versão estavél do n8n até o momento é 1.3.1, que necessita do Node.js v18.x

```bash
sudo npm install -g n8n
```

```bash
npm install pm2 -g
```

```bash
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
```

```bash
sudo apt install ./google-chrome-stable_current_amd64.deb
```

```bash
sudo nano /etc/nginx/sites-available/n8n
```

```bash
server {
  server_name conector.dominio.com.br;
  
  underscores_in_headers on;

  location / {

   proxy_pass http://127.0.0.1:5678;
   proxy_pass_header Authorization;
   proxy_set_header Upgrade $http_upgrade;
   proxy_set_header Connection "upgrade";
   proxy_set_header Host $host;
   proxy_set_header X-Forwarded-Proto $scheme;
   proxy_set_header X-Forwarded-Ssl on; # Optional
   proxy_set_header X-Real-IP $remote_addr;
   proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
   proxy_http_version 1.1;
   proxy_set_header Connection "";
   proxy_buffering off;
   client_max_body_size 0;
   proxy_read_timeout 36000s;
   proxy_redirect off;
  }
  add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
  ssl_protocols TLSv1.2 TLSv1.3;
} 
  ```

```bash
sudo ln -s /etc/nginx/sites-available/n8n /etc/nginx/sites-enabled
```

```bash
sudo certbot --nginx
```

```bash
sudo service nginx restart
```

```bash
pm2 start n8n --cron-restart="0 0 * * *" -- start
```

### Execute esse comando abaixo para não cair seu n8n quando você reiniciar sua VPS

```bash
sudo pm2 startup ubuntu -u root && sudo pm2 startup ubuntu -u root --hp /root && sudo pm2 save
```

```bash
nano /root/.n8n/.env
```

Altere as seguintes variaveis baixo no arquivo `.env`

DB_POSTGRESDB_PASSWORD=SenhaAqui

C8Q_QP_DEFAULT_USER=coloque email do Quepasa

C8Q_QP_BOTTITLE=Nome do seu site

C8Q_CW_PUBLIC_URL=domniochatwoot

C8Q_QP_CONTACT=Seu email

C8Q_QP_DEFAULT_USER=Seu email

WEBHOOK_URL=https://conector.dominio.com.br

N8N_EDITOR_BASE_URL=https://conector.dominio.com.br

```
DB_TYPE=postgresdb
DB_POSTGRESDB_HOST=localhost
DB_POSTGRESDB_PORT=5432
DB_POSTGRESDB_USER=n8n_user
DB_POSTGRESDB_PASSWORD=SenhaAqui
DB_POSTGRESDB_DATABASE=n8n_db
C8Q_SINGLETHREAD=false
C8Q_QUEPASAINBOXCONTROL=1001
C8Q_GETCHATWOOTCONTACTS=1002
C8Q_QUEPASACHATCONTROL=1003
C8Q_CHATWOOTPROFILEUPDATE=1004
C8Q_POSTTOWEBCALLBACK=1005
C8Q_POSTTOCHATWOOT=1006
C8Q_CHATWOOTTOQUEPASAGREETINGS=1007
C8Q_CW_PUBLIC_URL="chatwoot.seudominio.com.br"
C8Q_QP_DEFAULT_USER="contato@seudominio.com.br"
C8Q_QP_BOTTITLE="Chatwoot"
C8Q_QP_CONTACT="contato@seudominio.com.br"
N8N_EDITOR_BASE_URL="https://conector.dominio.com.br"
WEBHOOK_URL="https://conector.dominio.com.br"
EXECUTIONS_DATA_PRUNE=true
EXECUTIONS_DATA_MAX_AGE=168
EXECUTIONS_DATA_PRUNE_MAX_COUNT=5000
GENERIC_TIMEZONE="America/Sao_Paulo"
```

### Cria um link simbólico chamado ".env" que aponta para o arquivo "./.n8n/.env" no sistema de arquivos.

```bash
ln -s ./.n8n/.env .env
pm2 restart all --update-env
```

OBS: Não crie sua conta agora, antes de instalar API Quepasa!

<
