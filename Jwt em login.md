---
tags:
  - Segurança
  - Nest
aliases: 
lead: Aprendi a introduzir niveis de login na aplicação nest usando jwt
created: 2025-04-02, 15:37
modified: 2025-04-02, 15:37
template_type: Nota
template_version: "1.35"
license: MicksCoder
---


# Jwt em login

> [!Resumo]
> `Nessa nota vamos ver a estrutura em geral para depois especificarmos em cada passo, deve-se entender o conteudo em geral`

# **PRINCIPAL**
-  Basicamente no nest são client server, o front e o back, o front deve mostrar pro back que ele tem a senha entende? senha essa que vai ser o [[Jwt]] e entao determinamos um prazo de validade para tal, vamos ter em passo o:
	- Gerar o token
	- Validar o token
	- Devolver o token
	- Front salva no lado do cliente apenas
	- Acesso a aplicação

- Otimo, agora como o nest faz isso, ele basicamente tem um guardião que define a estrategia,essa estrategia tem varias possibilidades, vamos aprender a padrão aqui no artigo da documentação do nest abaixo:

 -  Prepare o prisma e o banco de dados a ser usado
 - Instale as dependências 
 - 

**Conteúdo de Apoio**
- https://youtu.be/3z6Cs_PtYc0?si=p5pKIVIDKHCeH_ap

---
# Complementos

**Fonte**
- baseado_em:: https://youtu.be/3z6Cs_PtYc0?si=p5pKIVIDKHCeH_ap

**Referências**
- veja:: https://docs.nestjs.com/security/authentication

**Termos**
- 

**Destino**
- usado_em:

---
**Tarefas**
- 

**Perguntas**
- pergunta

---
**Ajuda do Modelo**
- [Estrutura Básica do Modelo](https://github.com/groepl/Obsidian-Templates#basic-template-structure)
- [Como Usar Links](https://github.com/groepl/Obsidian-Templates#how-to-use-links)
- [Como Usar Tags](https://github.com/groepl/Obsidian-Templates#how-to-use-tags)
- [Como Pesquisar Notas](https://github.com/groepl/Obsidian-Templates#how-to-search-notes)
- [Plugins Necessários](https://github.com/groepl/Obsidian-Templates#obsidian-plugins-needed)
- [Encontrar as Últimas Atualizações](https://github.com/groepl/Obsidian-Templates)
