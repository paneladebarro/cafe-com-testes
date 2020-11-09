<p align="center">
  <a href="https://github.com/pagarme/cafe-com-testes">
    <img src="../.github/cafecomtestes.png" alt="Café com Testes">
  </a>
</p>

# Variáveis de ambiente no Cypress

Frequentemente precisamos escrever testes e2e para verificação de processos que solicitam recursos protegidos ou que exigem credenciais apropriadas. Por exemplo, um formulário de login que espera um nome de usuário e senha válidos, um terminal que usa uma chave API ou uma solicitação HTTP que precisa de um token verificável. Uma das abordagens comuns é salvar os dados protegidos como variáveis de ambiente.

O Cypress oferece várias maneiras de trabalhar com variáveis de ambiente. Em sua [documentação](https://docs.cypress.io/guides/guides/environment-variables.html#Setting) sobre variáveis de ambiente são listadas diferentes opções demonstrando o seu uso, e também prós e contras de cada abordagem.

# Por que precisamos de uma variável de ambiente?

Variáveis de ambiente são muito úteis quando:

1. Os valores são diferentes nas máquinas do desenvolvedor;
2. Os valores são diferentes em vários ambientes: dev, staging, qa, prod, hml, sandbox;
3. Os valores são altamente dinâmicos e mudam com frequência;
4. Você pode alterar facilmente as variáveis de ambiente, especialmente quando estiver executando em CI;
5. Você está lidando com credenciais que não podem ser expostas por segurança;

Ao invés de colocar diretamente no seu código

```javascript
cy.request('https://api.acme.corp') // this will break on other environments`
```

Você pode mover para uma variável de ambiente Cypress como exemplificado abaixo, em que foi criada uma variável de ambiente chamada EXTERNAL_API deixando o código com uma melhor manutenibilidade e liberdade de execução em diferentes ambientes.

```javascript
cy.request(Cypress.env('EXTERNAL_API')) // this will point to a dynamic env var
```

# Setting

Existem diferentes formas de definir variáveis de ambiente. Cada uma delas tem um caso de uso ligeiramente diferente.E nesse tutorial vamos nos concentrar em duas opções: configuration file e cypress.env.json. Você não deve se sentir obrigado a escolher apenas um método, inclusive na documentação você terá acesso a outros métodos.

Quando você executa seus testes, você pode usar a função Cypress.env para acessar os valores de suas variáveis de ambiente.

## Opção nº 1 Arquivo de configuração 

Qualquer par de chave/valor que você definir em seu arquivo de configuração (cypress.json por padrão) na chave env se tornará uma variável de ambiente.

```json
{
  "projectId": "128076ed-9868-4e98-9cef-98dd7a705d75",
  "env": {
    "login_url": "/login",
    "login_user": "usuariomaster",
    "login_password": "Senha123"
  }
}
```
E no arquivo de teste você chamará as variáveis da seguinte maneira:

```javascript
Cypress.env()      // { "login_url": "/login","login_user":"usuariomaster","login_password": "Senha123"}
Cypress.env('login_url')  // '/login'
Cypress.env('login_user') // 'usuariomaster'
Cypress.env('login_password') // 'Senha123'

```

### Benefícios

Essa opção é ótima para valores que precisam ser verificados no controle de origem e permanecem os mesmos em todas as máquinas.

### Desvantagens

A desvantagem dessa opção é que ela só funciona para valores que devem ser iguais em todas as máquinas.

## Opção nº 2 cypress.env.json

Alternativamente, você pode decidir criar seu próprio arquivo cypress.env.json que o Cypress verificará automaticamente. Os valores neste arquivo sobrescreverão variáveis de ambiente conflitantes em seu arquivo de configuração (cypress.json por padrão).

Esta é uma estratégia muito útil porque, se você adicionar cypress.env.json ao seu arquivo .gitignore, os valores no arquivo podem ser diferentes para cada máquina de desenvolvedor ou ambiente sem nenhum problema.

```json
{
  "host": "testandoapi.dev.local",
  "api_server": "http://localhost:8080/api/v1/"
}
```
E no arquivo de teste você chamará as variáveis da seguinte maneira:

```javascript
Cypress.env()             // {host: 'testandoapi.dev.locall', api_server: 'http://localhost:8888/api/v1'}
Cypress.env('host')       // 'testandoapi.dev.local'
Cypress.env('api_server') // 'http://localhost:8888/api/v1/'

```

### Benefícios

1. Esta opção fornece um arquivo dedicado apenas para variáveis de ambiente.
2. Isso permitirá que você gere este arquivo a partir de outros processos de construção.
3. Os valores podem ser diferentes em cada máquina (se não for verificado no controle de origem).
4. Esta opção suporta campos aninhados (objetos), por exemplo, {testUser: {nome: '...', email: '...'}}.


### Desvantagens

1. Este é outro arquivo com o qual você tem que lidar.
2. Pode ser um exagero para 1 ou 2 variáveis de ambiente.
