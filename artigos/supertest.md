<p align="center">
  <a href="https://github.com/pagarme/cafe-com-testes">
    <img src="../.github/cafecomtestes.png" alt="Café com Testes">
  </a>
</p>

# Utilizando SuperTest para os testes de Integração

[SuperTest](https://github.com/visionmedia/supertest) é uma lib agnóstica à framework que tem como objetivo facilitar os testes usando HTTP de uma maneira simples e abstrata. Caso seja necessário, a lib também permite o uso mais baixo nível para um maior controle dos testes.

# Quais o benefícios ? 

Utilizando uma lib para "mockar" o próprio serviço, você consegue testar as rotas de uma maneira mais fácil com o foco na regras de negócio, pois a abstração do serviço já foi resolvida. Além disso, remove a necessidade de ter containers para executar os testes de integração, diminuindo qualquer chance de intermitência dos testes devido a alguma configuração errada do serviço.

# Prática 

## Sem o Uso do superTest
```javascript
  const fetch = require('node-fetch');
  const endpoint = 'http://api:3000'
  // necessidade de ter uma resolução de request real
  it('POST /teste should respond successfully with status 200 and request body as expected', async () => {
    // request é uma função qualquer que abstrai o http request 
    const response = await fetch(
      `${endpoint}/teste`,
      method: 'post',
      body: JSON.stringify({ name: 'body qualquer' }),
      headers: { 'Content-Type': 'application/json' }
    )

    const body = await response.json()

    expect(response.status).toBe(200)
    expect(body.name).toBe('body qualquer')
  })
```

Observações:

- É necessário rodar o serviço, seja utilizando o node diretamente ou um container escutando a porta 3000
- É necessário o uso de request real
- Em caso de containers sendo utilizados, chamadas internas da api de I/O não poderão ser mockadas. As chamadas de I/O interceptáveis no escopo
de teste são apenas as chamadas para a própria API.

## Com uso de superTest
```javascript

const request = require('supertest')
const express = require('express')

describe('/test endpoint test cases', () => {
  const app = express()

  it('POST /teste should respond successfully with status 200 and request body as expected', async () => {
  app.post('/teste', myHandler)

  return request(app)
    .post('/teste')
    .send({ name: 'body qualquer' })
    .expect('Content-Type', /json/)
    .expect(200)
    .then(response => {
      const { body } = response
      expect(body.name).toBe('qualquer coisa')
    })
  })
})
```

Observações:

- Não há necessidade de levantar um serviço real
- Requests aceita qualquer `http.Server` ou função
- Possibilidade de inclusão de todos os middlewares caso queira
- Gerar mocks de chamadas internas da API, pois são capturáveis no escopo do teste
