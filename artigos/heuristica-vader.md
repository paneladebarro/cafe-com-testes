<p align="center">
  <a href="https://github.com/pagarme/cafe-com-testes">
    <img src="../.github/cafecomtestes.png" alt="Café com Testes">
  </a>
</p>

# Heurística VADER para testes de Rest API

Heurísticas são atalhos cognitivos que nos auxiliam a resolver problemas que outras pessoas já passaram e mapearam como resolver. Em teste de software temos várias heuríticas sobre como testar diferentes aplicações. Uma delas é a VADER para testes de API Rest.

VADER -> Verbos, Autorização/Autenticação, Dados, Erros e Responsividade.

Seguindo essa heurística já sabemos o que precisamos testar dentro de cada um desses grupos:

## V - Verbos

Testar todos os verbos disponíveis na API, por exemplo POST, PUT, PATCH, DELETE, GET, OPTIONS

## A - Autorização/Autenticação

Testar se os recursos estão disponíveis apenas mediante ao uso de um token ou API Key, se essas informações são passadas no Header ou via querystring na URL, e também os casos de exceção com usuários inválidos e/ou inexistentes.

## D - Dados

Testar a serialização dos dados, tipo, formato, regras de tamanho e paginação

## E - Erros

Aqui testamos os diferentes códigos de erro e os payloads considerando os cenários de exceção.

## R - Responsividade

Por fim validamos os timeouts e concorrência.

<p align="left">
  <a href="https://github.com/pagarme/cafe-com-testes">
    <img src="../.github/vader.png" width="300" alt="Café com Testes">
  </a>
</p>

Para saber mais:
* [VADER – a REST API test heuristic](http://qa-matters.com/2016/07/30/vader-a-rest-api-test-heuristic/)
* [API Testing Heuristics for Developers](https://europeantestingconference.eu/slides18/Roy.pdf)
