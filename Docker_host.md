---
tags:
  - tipo/nota
  - tema/xyz
aliases: 
lead: +++ Parágrafo de introdução vai aqui +++
created: 2025-04-15, 16:24
modified: 2025-04-15, 16:24
template_type: Nota
template_version: "1.35"
license: MicksCoder
---


# Docker_host

> [!Resumo]
> `= this.lead`

# **PRINCIPAL**
# 🐳 DOCKER_HOST

`DOCKER_HOST` define **onde** o cliente Docker envia comandos.

Por padrão, aponta para o Docker local (`unix:///var/run/docker.sock`).  
Mas pode ser configurado para acessar **outro servidor**:

```bash
export DOCKER_HOST=ssh://usuario@IP
```

Assim, comandos como `docker ps` e `docker compose up -d` são executados remotamente.

---

## 🧠 Para que serve?

- Controlar o Docker de um **servidor remoto**
- Automatizar **deploys com GitHub Actions**
- Evitar login manual ou copiar arquivos

---

## 🌐 Formatos comuns

- Local: `unix:///var/run/docker.sock`
- SSH: `ssh://arthur@meu-servidor.com` ← recomendado
- TCP: `tcp://IP:2375` ← inseguro se sem TLS

---

## 🛡️ Segurança

Evite usar TCP aberto.  
Prefira conexões via `ssh://` ou **[[Docker TLS]]**.

---

## 🧪 Testar conexão

```bash
docker info
```

Vai mostrar o Docker do servidor remoto.

---

## 🔗 Links relacionados

- [[Deploy com Docker Compose]]
- [[Acesso remoto com SSH]]
- [[Docker TLS]]
- [[CI/CD com GitHub Actions]]


