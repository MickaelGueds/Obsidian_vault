# Tutorial: Usando NGINX como Proxy Reverso para Múltiplos Domínios com HTTPS via Let's Encrypt

## 🌐 Objetivo

Criar um ambiente com Docker onde múltiplos domínios passam por um NGINX central, que atua como proxy reverso com redirecionamento HTTPS usando certificados gratuitos do Let's Encrypt. Os sites já estão rodando em instâncias separadas de Docker Compose.

---

## 1. 🔐 Registrar e Apontar Domínios para o VPS

- Acesse o painel do seu provedor de domínio (ex: Hostinger, GoDaddy, Registro.br).
    
- Para cada domínio (ex: `site1.com`, `site2.com`), crie um **registro A**:
    aaaa

```
Tipo: A
Nome: @
Valor: <IP do seu VPS>
```

---

## 2. ⚙️ Estrutura com Docker para o Proxy

Você manterá seus sites rodando em seus próprios `docker-compose.yml` separados. O NGINX e o Certbot ficarão em um projeto à parte, como o proxy reverso.

Estrutura:

```
/proxy/
├── nginx/
│   └── nginx.conf
├── certbot/
└── docker-compose.yml (somente para nginx + certbot)
```

---

## 3. 📊 Configuração do NGINX com Redes Docker Compartilhadas e Segurança

Crie uma rede Docker externa chamada `nginx-proxy-net`:

```bash
docker network create nginx-proxy-net
```

No `docker-compose.yml` de cada serviço (front1, back1, etc.), adicione:

```yaml
networks:
  - nginx-proxy-net
```

No `docker-compose.yml` do proxy (abaixo), adicione a mesma rede.

Agora o `nginx.conf` pode usar diretamente os nomes dos serviços como `proxy_pass`.

**Inclua as seguintes práticas de segurança em todos os `server` blocks:**

- **Rate limiting** com `limit_req`
    
- **Cache control** para arquivos estáticos
    
- **CORS headers** para a API
    

```nginx
limit_req_zone $binary_remote_addr zone=one:10m rate=60r/s;

server {
    listen 80;
    server_name site1.com;
    server_tokens off;
    limit_req zone=one burst=100 nodelay;

    location /.well-known/acme-challenge/ {
        root /var/www/certbot;
    }

    location / {
        proxy_pass http://front1:80;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg)$ {
        expires 30d;
        add_header Cache-Control "public, no-transform";
    }

    location /api {
        proxy_pass http://back1:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_cache_bypass $http_upgrade;
        add_header 'Access-Control-Allow-Origin' '*' always;
        add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, OPTIONS' always;
        add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range,Authorization' always;
        add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range' always;
    }
}

server {
    listen 443 ssl;
    server_name site1.com;
    server_tokens off;
    limit_req zone=one burst=100 nodelay;

    ssl_certificate /etc/letsencrypt/live/site1.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/site1.com/privkey.pem;

    location / {
        proxy_pass http://front1:80;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg)$ {
        expires 30d;
        add_header Cache-Control "public, no-transform";
    }

    location /api {
        proxy_pass http://back1:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_cache_bypass $http_upgrade;
        add_header 'Access-Control-Allow-Origin' '*' always;
        add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, OPTIONS' always;
        add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range,Authorization' always;
        add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range' always;
    }
}
```

---

## 4. 🔒 Gerar Certificados com Certbot

Com o contêiner `certbot`, use:

```bash
certbot certonly \
  --webroot -w /var/www/certbot \
  -d site1.com -d www.site1.com

certbot certonly \
  --webroot -w /var/www/certbot \
  -d site2.com -d www.site2.com
```

---

## 5. ⏰ Renovação Automática com Cron

Para automatizar a renovação dos certificados SSL, adicione o seguinte cron job diretamente no servidor VPS (fora do Docker):

1. Acesse o servidor via SSH:
    

```bash
ssh usuario@seu-servidor
```

2. Edite o crontab do root:
    

```bash
sudo crontab -e
```

3. Adicione esta linha ao final do arquivo:
    

```bash
0 5 * * * docker exec certbot certbot renew && docker exec nginx nginx -s reload
```

Isso fará com que todos os dias, às 05:00 da manhã, os certificados sejam renovados automaticamente e o NGINX seja recarregado para aplicar as mudanças.

---

## 📦 Exemplo de docker-compose.yml (Somente Proxy)

```yaml
version: '3'

services:
  nginx:
    image: nginx:latest
    container_name: nginx-proxy
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
      - /etc/letsencrypt:/etc/letsencrypt
      - ./certbot/www:/var/www/certbot
    networks:
      - nginx-proxy-net

  certbot:
    image: certbot/certbot
    container_name: certbot
    volumes:
      - /etc/letsencrypt:/etc/letsencrypt
      - ./certbot/www:/var/www/certbot
    entrypoint: ""
    command: >
      sh -c "certbot certonly --webroot --webroot-path=/var/www/certbot \
      -d site1.com -d www.site1.com \
      -d site2.com -d www.site2.com \
      --email seuemail@exemplo.com --agree-tos --no-eff-email"
    networks:
      - nginx-proxy-net

networks:
  nginx-proxy-net:
    external: true
```

---

## ✅ Resultado Esperado

- Cada domínio acessa o NGINX
    
- NGINX reconhece e redireciona para o serviço correto já rodando com caminhos específicos
    
- HTTPS ativo com certificados da Let's Encrypt
    
- Renovação automática e gratuita
    

---

