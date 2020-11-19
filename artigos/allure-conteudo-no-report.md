<p align="center">
  <a href="https://github.com/pagarme/cafe-com-testes">
    <img src="../.github/cafecomtestes.png" alt="Café com Testes">
  </a>
</p>

# Allure adicionando conteúdo no report 

Você já sentiu falta de alguma informação no seu relatório de execução de testes? Se a resposta para essa pergunta for sim, temos a solução para o seu problema! Neste artigo iremos abordar as vantagens e como adicionar mais conteúdo no Allure Report.

> Como o Allure tem muito conteúdo, esse será o primeiro de uma série de artigos em que iremos abordá-lo.  


## Por que adicionar ou anexar conteúdo aos relatórios de execução dos testes?

Imagine aquele teste end-to-end, seja web ou mobile, que é intermitente e quebra aleatoriamente quando executado no CI, seria interessante ter um vídeo da execução para saber o que aconteceu, certo? Ou aquele teste de API, que você quer conferir qual foi exatamente o body de retorno. 

Tendo acesso a mais informações você consegue tomar decisões mais assertivas e confiáveis. 


## Chega de enrolar e bora para o código 

Como o Allure tem suporte para várias linguagens, e às vezes existem diferenças na integração, eu aconselho a olhar na [documentação](http://allure.qatools.ru/) como é feito especificamente para a linguagem/framework que você está utilizando.

Antes de mais nada, devemos pensar onde aquela informação será incorporada.

Pensando em um caso de teste padrão usando javascript, onde temos um 'Describe' para uma "suíte" de testes e um 'it' para cada caso de teste. Podemos utilizar o código em qualquer parte do caso teste:

```javascript
reporter.addAttachment(name: string, buffer: any, type: string)
```
Mesmo exemplo em Ruby:

```ruby
Allure.add_attachment(name: "attachment", source: "Some string", type: Allure::ContentType::TXT, test_case: false)
```

* o primeiro parâmetro é o nome que vai aparecer no anexo, aqui você pode usar qualquer _string_
* o segundo se refere ao conteúdo, que pode ser um texto, uma imagem, um vídeo, um CSV, entre outros. 
* o terceiro é o tipo do arquivo que você está anexando, em alguma linguagens existe um _enum_ que facilita esse ponto.

Você também pode adicionar essa linha de código dentro do 'AfterEach' e os anexos irão ser adicionados dentro dos resultados de cada um dos seus testes. 

Exemplo:
```javascript jest
  afterEach(() => {
    reporter.addAttachment('body', JSON.stringify(response['body']), 'text/json')
  })
```
Desta forma não será obrigatório colocar o "addAttachment" em cada teste, e sim apenas uma vez. 
Lembrando, que 'body' é o nome que será dado ao seu anexo, e o 'response['body']' é uma variável global do teste, que fica reciclando com o resultado da reposta da chamada de API.

> Não será necessário fazer nenhum import no Jest, porque é feita a injeção em tempo de execução.

Já quem utilizar o BDD como guia de execução dos testes pode adicionar o conteúdo dentro da Feature, utilizando o afterFeature que alguns frameworks dão suporte. 
Dentro do cenário usando um 'afterScenario' o anéxo ira para o final dos passos.

### O que anexar ? 

Aqui vai uma dica de tipos de arquivos que podemos adicionar:

* "text/plain"
* "application/xml"
* "text/csv"
* "text/tab-separated-values"
* "text/css"
* "text/uri-list"
* "image/svg+xml"
* "image/png"
* "application/json"
* "video/webm"
* "image/jpeg"


### Exemplo

Na imagem a baixo podemos observar um teste que falhou, e no body do teste temos os anexos de vários tipos, CSV e JPG por exemplo.

<p align="center">
    <img src="https://docs.qameta.io/allure/images/testcase.png" alt="Exemplo">
</p>


 Exemplo de report completo do Allure
 https://demo.qameta.io/allure/#suites/1bbdddb8b139ac506f125d271028f72a/ca0bdae7438b29d/


 Link do projeto para o Jest que não é suportado oficialmente
 https://github.com/zaqqaz/jest-allure


 ## Benefícios 
  
* Ter mais informações, que já são geradas nos testes; 
* Centralizar as informações em um único lugar;

 ## Cuidados

  Você literalmente pode armazenar tudo e isso vai fazer com que você tenha problemas com espaço dependendo de onde isso for armazenado. 
