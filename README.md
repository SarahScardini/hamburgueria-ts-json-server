<h1 align="center">
  <img alt="X-Burguer" title="Hamburgueria 2.0" src='https://i.ibb.co/djbw6LV/x-burgue.png' width="100px" />
</h1>

<h1 align="center">
   Hamburgueria 2.0 - API
</h1>

<p align = "center">
Este é a documentação para utlizar a API da Hamburgueria 2.0. Uma API voltada para e-commerce de alimentos. 
</p>

## **Endpoints**

O usuário pode cadastrar seu perfil, adicionar itens no carrinho se quiser comprar no momento ou salvar para mais tarde.

O url base da API é https://hamburgueria-ts-json-server.herokuapp.com/

<br/>\*\*\*\*

## Rotas que não precisam de autenticação

<h2 align ='center'> Acessando os produtos </h2>

Nesse endpoint todos tem acesso a leitura, logado ou não. Na API podemos acessar a lista de produtos dessa forma:

`GET /products - FORMATO DA RESPOSTA - STATUS 200`

```json
[
  {
    "id": 1,
    "name": "Hamburguer",
    "category": "Sanduíches",
    "price": 14,
    "img": "https://i.ibb.co/fpVHnZL/hamburguer.png"
  },
  {
    "id": 2,
    "name": "X-Burguer",
    "category": "Sanduíches",
    "price": 16,
    "img": "https://i.ibb.co/djbw6LV/x-burgue.png"
  }
]
```

Se preferir pode utilizar os query params para filtrar a lista. Por exemplo, para filtrar pela categoria fazemos da seguinte maneira:

`GET /products?category=Combos - FORMATO DA RESPOSTA - STATUS 200`

```json
[
  {
    "id": 16,
    "name": "Combo Kenzie",
    "category": "Combos",
    "price": 26,
    "img": "https://i.ibb.co/TtjMVRz/combo.png"
  }
]
```

Podemos pesquisar diretamente pelo id da produto:

`GET /products/:id - FORMATO DA RESPOSTA - STATUS 200`

```json
{
  "id": 7,
  "name": "Batata Frita",
  "category": "Acompanhamentos",
  "price": 9.5,
  "img": "https://i.ibb.co/jT04ZHb/fries.png"
}
```

<h2 align ='center'> Criação de usuário </h2>

<p>Para criar um cadastro, pode usar qualquer uma das 3 opções: </p>
<ul>
<li><b>POST /register</b></li> 
<li><b>POST /signup</b></li> 
<li><b>POST /users</b></li> 
</ul>

`POST /users - FORMATO DA REQUISIÇÃO`

```json
{
  "name": "London",
  "password": "000000",
  "email": "london@email.com"
}
```

Caso dê tudo certo, a resposta será assim:

`POST /users - FORMATO DA RESPOSTA - STATUS 201`

```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6ImxvbmRvbkBlbWFpbC5jb20iLCJpYXQiOjE2NDMzNjkxNTcsImV4cCI6MTY0MzM3Mjc1Nywic3ViIjoiMSJ9.CLqO01mGQf6C3j5iA7YyxIZof6ff4dJdH9n7tVvr7pY",
  "user": {
    "email": "london@email.com",
    "name": "London",
    "id": 1
  }
}
```

<p>O email e senha são obrigatórios, as outras informações são opcionais.</p>

<h2 align ='center'> Possíveis erros </h2>

Caso você se esqueça de passar alguma informação obrigatória, retornará a seguinte mensagem:

`POST /users - `
` FORMATO DA RESPOSTA - STATUS 400`

```json
"Email and password are required"
```

A senha necessita de pelo menos 4 caracteres.

`POST /users - `
` FORMATO DA RESPOSTA - STATUS 400`

```json
"Password is too short"
```

Email já cadastrado:

`POST /users - `
` FORMATO DA RESPOSTA - STATUS 400`

```json
"Email already exists"
```

<br/>

<h2 align = "center"> Login </h2>

<p>Para fazer o login pode usar qualquer uma das 2 opções: </p>
<ul>
<li><b>POST /login</b></li> 
<li><b>POST /signin</b></li> 
</ul>

`POST /login - FORMATO DA REQUISIÇÃO`

```json
{
  "email": "london@email.com",
  "password": "000000"
}
```

Caso dê tudo certo, a resposta será assim:

`POST /login - FORMATO DA RESPOSTA - STATUS 201`

```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6ImxvbmRvbkBlbWFpbC5jb20iLCJpYXQiOjE2NDMzNjk2MjAsImV4cCI6MTY0MzM3MzIyMCwic3ViIjoiMSJ9.R8byd_pE0JPwHeiYK_zqKJcB8MtWxjBsZHMhJm5L6rg",
  "user": {
    "email": "london@email.com",
    "name": "London",
    "id": 1
  }
}
```

Com essa resposta, vemos que temos duas informações, o user e o token respectivo, dessa forma você pode guardar o token e o usuário logado no localStorage, por exemplo, para fazer a gestão do usuário no seu front-end.
<br/>

\*\*importante: No endpoint users, apenas o usuário que criou o cadastro tem acesso a leitura e escrita do mesmo se estiver logado.
<br/>
<br/>

## Rotas que necessitam de autorização

Rotas que necessitam de autorização deve ser informado no cabeçalho da requisição o campo "Authorization", dessa forma:

> Authorization: Bearer {token}

<br/>

<h2 align ='center'> Adicionar produto ao carrinho </h2>

Essa rota apenas o usuário que adicionou o produto ao carrinho, tem acesso a leitura e escrita.

`POST /cart - FORMATO DA REQUISIÇÃO`

```json
{
  "userId": 4,
  "products": [
    {
      "id": 10,
      "name": "Sorvete",
      "category": "Sobremesa",
      "price": 10,
      "img": "https://i.ibb.co/hW9nzvF/sorvete.png",
      "quantity": 1
    }
  ]
}
```

O campo "userId" é obrigatório. Ao adicionar com sucesso uma id será gerada:
`POST /cart - FORMATO DA REQUISIÇÃO`

```json
{
  "userId": 4,
  "products": [
    {
      "id": 10,
      "name": "Sorvete",
      "category": "Sobremesa",
      "price": 10,
      "img": "https://i.ibb.co/hW9nzvF/sorvete.png",
      "quantity": 2
    }
  ],
  "id": 2
}
```

Caso você tente adicionar um produto sem a id do usuário receberá este erro:

`POST /cart - FORMATO DA RESPOSTA - STATUS 401`

```json
"Private resource creation: request body must have a reference to the owner id"
```

Para atualizar, pode se usar tanto o PATCH quanto o PUT:

<ul>
<li><b>PATCH /cart</b></li> 
<li><b>PUT   /cart</b></li> 
</ul>

`PUT /cart/:cart_id - FORMATO DA REQUISIÇÃO`

```json
{
  "userId": 4,
  "products": [
    {
      "id": 10,
      "name": "Sorvete",
      "category": "Sobremesa",
      "price": 10,
      "img": "https://i.ibb.co/hW9nzvF/sorvete.png",
      "quantity": 2
    },
    {
      "id": 11,
      "name": "Lanche Vegano - Molho de Mostarda",
      "category": "Sanduíches",
      "price": 17,
      "img": "https://i.ibb.co/YDnB1wz/vegano-com-molho-de-mostarda.png",
      "quantity": 1
    }
  ],
  "id": 2
}
```

Também é possível deletar o carrinho:

`DELETE /cart/:cart_id`

```
Não é necessário um corpo da requisição.
```

<br/>
<br/>

<h2 align ='center'> Atualizando os dados do perfil </h2>

O usuário precisa estar logado e ter o token no cabeçalho da requisição. Estes endpoints são para adicionar, atualizar ou alterar seus dados com qualquer outra informação que julgar necessária.
Lembrando que pode se usar tanto o PATCH quanto PUT:

`PATCH /users/:user_id - FORMATO DA REQUISIÇÃO`

```json
{
  "email": "london@mail.com",
  "password": "1111",
  "name": "London"
}
```

Caso dê tudo certo, a resposta será assim:

`PATCH /users/:user_id - FORMATO DA RESPOSTA - STATUS 201`

```json

{
  "email": "london@mail.com",
  "password": "$2a$10$hNdl84.UQUN1nf/3EmTLM.tQ9W0bj.ET02.f6OiMTfzLE4kMa0cdG",
  "name": "London"
}****
```

---

Feito com ♥ by SarahScardini
