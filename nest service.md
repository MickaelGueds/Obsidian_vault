---
tags:
  - Backend
  - Service
aliases: 
lead: Services do nest e o que eu aprendi
created: 2025-04-30, 07:52
modified: 2025-04-30, 07:52
template_type: Nota
template_version: "1.35"
license: MicksCoder
---


# nest service

# O que é um service?
- Basicamente um service aqui vai ser responsável pela logica de negócios, não lida com o http mas com ***regras, manipulação de dados, integrações e etc...os métodos sempre vem daqui*** baseados sempre e pensando na reutilização e é usado como injeção de dependências no nestJS 

# Boas praticas:

## Single responsibility
- cada service cuida de uma coisa apenas, não tem logica de http ok?
## Retorna dados ou exceções
- É o service que retorna os dados ou ate exceções pro [[nest controller]] tratar
## Dto
- Use Data Transfer Object, que é a classe que define a estrutura e tipagem dos dados que você vai receber no service, você valida e melhora a segurança, contribuindo para uma boa documentação, voce fala se o Objeto tem que ter algo e qual o tipo desse algo
## Métodos
- Sempre defina o tipo de parâmetro e o retorno deles
- `async create(createBookDto: CreateBookDto): Promise<Book>`

# Como decidir os parâmetros? 
## 1 - Qual o objetivo do metodo
Pergunte-se:

- O que esse método precisa fazer? (Criar, buscar, atualizar, deletar, filtrar, etc)

- O que é necessário para realizar essa ação? (Dados do usuário, ID, filtros, etc)
## 2 - O que precisa receber
- Criação: Precisa dos dados do novo recurso (ex: título, autor, etc).

→ Use um DTO para garantir formato e validação.

- Busca única: Precisa de um identificador único (ex: id, email, etc).

- Busca múltipla: Precisa de filtros, paginação, ordenação (ex: title, skip, take, orderBy).

- Atualização: Precisa do identificador + dados a atualizar (ex: id + DTO).

- Remoção: Precisa do identificador.

Dica:

Se o dado vem do corpo da requisição, use DTO.

Se vem da URL, geralmente é um parâmetro simples (string, number).

## 3 - Como decidir o tipo
- DTO: Para dados complexos (criação, atualização, filtros).

- string/number: Para identificadores simples (id, email).

- Objeto: Para filtros mais flexíveis (ex: { skip, take, where }).

## 4 - Aprenda a fazer querys, dentro de métodos do prisma por exemplo 
- Criação:

Use prisma.model.create({ data: ... })

Passe o DTO diretamente (ou adapte se necessário).

- Busca única:

Use prisma.model.findUnique({ where: { id } })

O parâmetro where recebe um objeto com o identificador.

- Busca múltipla:

Use prisma.model.findMany({ where, skip, take, orderBy })

Monte o objeto where com base nos filtros recebidos.

- Atualização:

Use prisma.model.update({ where: { id }, data: ... })

Recebe o id e o DTO de atualização.

- Remoção:

Use prisma.model.delete({ where: { id } })

## 5 - Exemplos:
- Criação: Recebe DTO, usa create.

- Busca única: Recebe id, usa findUnique.

- Busca múltipla: Recebe filtros, usa findMany.

- Atualização: Recebe id + DTO, usa update.

- Remoção: Recebe id, usa delete.

**Conteúdo de Apoio**
- 

---
# Complementos


- usado_em: [[nest controller]]

---
**Tarefas**
- 

**Perguntas**
- pergunta:: 

---
**Ajuda do Modelo**
- [Estrutura Básica do Modelo](https://github.com/groepl/Obsidian-Templates#basic-template-structure)
- [Como Usar Links](https://github.com/groepl/Obsidian-Templates#how-to-use-links)
- [Como Usar Tags](https://github.com/groepl/Obsidian-Templates#how-to-use-tags)
- [Como Pesquisar Notas](https://github.com/groepl/Obsidian-Templates#how-to-search-notes)
- [Plugins Necessários](https://github.com/groepl/Obsidian-Templates#obsidian-plugins-needed)
- [Encontrar as Últimas Atualizações](https://github.com/groepl/Obsidian-Templates)
