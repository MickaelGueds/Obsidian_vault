

# Fluxo de Autenticação com JWT e Bcrypt

Este documento descreve o fluxo completo de autenticação usando JWT (JSON Web Token) e bcrypt para uma aplicação que precisa identificar com segurança qual usuário realizou determinada ação (ex: pegar um carro emprestado).

---

## 1. Cadastro de Usuário (Sign-up)

1. O cliente envia **e-mail** e **senha** para o backend.
2. O servidor aplica `bcrypt.hash(senha, saltRounds)` para gerar um hash seguro da senha.
3. Apenas o **hash** da senha é salvo no banco de dados (nunca a senha em texto puro).

---

## 2. Login do Usuário (Sign-in)

1. O cliente envia **e-mail** e **senha**.
2. O servidor busca o usuário no banco e compara a senha enviada com o hash usando `bcrypt.compare`.
3. Se a senha for válida, o servidor gera um **access token JWT**, com payload como:

```json
{
  "sub": "id-do-usuario",
  "role": "admin",
  "iat": 1714670000,
  "exp": 1714670600
}
```

4. O token é assinado com uma **chave secreta** (`JWT_SECRET`) e enviado ao cliente.

---

## 3. Armazenamento do Token

O token pode ser armazenado pelo cliente de forma segura:

- **Web**:
  - `HttpOnly Cookie`: mais seguro contra XSS.
  - `localStorage`: mais prático, mas menos seguro.

- **Mobile**:
  - `SecureStorage`, `Keychain` (iOS) ou `Keystore` (Android).

---

## 4. Requisições Autenticadas

- Toda requisição protegida à API deve conter o header:

```
Authorization: Bearer <access-token>
```

- O backend intercepta a requisição e verifica o token com:

```ts
jwt.verify(token, JWT_SECRET)
```

Se válido, os dados do usuário são extraídos.

---

## 5. Identificação do Usuário

- Após validação do token, o backend acessa `req.user.sub` (ou `req.user.id`) para saber quem fez a requisição.
- Assim, é possível **registrar ações**, como:

> “Usuário 42 pegou o carro X às 14h”.

---

## 6. Autorização

Com base nos dados do payload (ex: `role`, `permissions`), o backend decide se o usuário pode ou não acessar determinado recurso.

---

## 7. Expiração e Renovação de Token

- O **access token** tem validade curta (ex: 10 minutos).
- Um **refresh token** pode ser emitido com validade maior (ex: 7 dias).
- Quando o access token expira, o cliente envia o refresh token para `/refresh` e recebe um novo access token.

---

## 8. Logout

- O frontend remove os tokens armazenados.
- O backend pode:
  - Adicionar o refresh token a uma **lista de revogação**.
  - Alterar a versão da sessão do usuário (ex: `token_version`).

---

## Segurança Recomendada

- Sempre usar **HTTPS**.
- **Não armazenar dados sensíveis** no token.
- Usar algoritmos seguros: `HS256` (com chave forte) ou `RS256` (assimétrico).
- Implementar **rotação de refresh token** e proteção contra **replay**.
- Registrar logs de atividades e falhas de autenticação para **auditoria**.

---

## Exemplo Prático com Express.js

```ts
import bcrypt from "bcrypt";
import jwt from "jsonwebtoken";
import { Request, Response } from "express";

const JWT_SECRET = process.env.JWT_SECRET!;
const ACCESS_TTL = "10m";

export async function login(req: Request, res: Response) {
  const { email, password } = req.body;
  const user = await db.user.findUnique({ where: { email } });
  if (!user || !(await bcrypt.compare(password, user.passwordHash))) {
    return res.status(401).json({ message: "Credenciais inválidas" });
  }

  const accessToken = jwt.sign(
    { sub: user.id, role: user.role },
    JWT_SECRET,
    { expiresIn: ACCESS_TTL }
  );

  res.json({ accessToken });
}

export function authGuard(req: Request, res: Response, next: Function) {
  const token = req.headers.authorization?.split(" ")[1];
  try {
    req.user = jwt.verify(token!, JWT_SECRET);
    return next();
  } catch {
    return res.status(401).json({ message: "Token inválido ou expirado" });
  }
}

app.post("/carros/:id/emprestar", authGuard, async (req, res) => {
  const carroId = req.params.id;
  await db.movimentacao.create({
    data: {
      carroId,
      usuarioId: req.user.sub,
      acao: "EMPRESTAR"
    }
  });
  res.status(204).send();
});
```

---

## Conclusão

Este fluxo garante:

- **Autenticação segura** com hash de senha.
- **Autorização baseada em papel**.
- **Identificação confiável** de quem executa cada ação.
- **Rastreabilidade** sem necessidade de sessão no servidor.

Ideal para aplicações modernas, stateless, com alto controle de acesso e auditabilidade.