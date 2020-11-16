<p align="center">
  <a href="https://github.com/pagarme/cafe-com-testes">
    <img src="../.github/cafecomtestes.png" alt="Café com Testes">
  </a>
</p>

# Allure adicionando conteúdo no report 

Olá a todos você sempre sentiu uma falta no report dos seus testes? , achou que faltava alguma informação ? Então seus problemas acabaram. Neste artigo iremos abordar as vantagens e como adicionar mais conteúdo no Allure. 

*Como o allure é muito grande esse será 1 de alguns artigos que iremos abordar. 


## Porque adicionar ou anexar conteúdo aos report dos testes

Imagina aquele teste End to End, mobile, aque é intermitênte rodando no CI, seria interessante ter um video disso correto ?, ou aquele teste de api que você quer conferir o que esta retorando no BODY ? 

Com o Volume maior de informações decisões se tornam mais acertivas e fáceis. 


## Parar de enrolar e bora para o código 

Como o Allure tem suporte para varias linguagens e as vezes existem um pequeno problema de integração então aconcelho a olhar na documentação como é feito especificamente para a linguagem/framework que esta utilizando. [documentação](http://allure.qatools.ru/)

Antes de mais nada deveremos pensar onde aquela informação será incorporada,

Pensando em um caso de teste padrão usando javascript onde temos um 'Describe' para uma "suite" de testes e um 'it' para cada teste, e queremos uma informação da suite inteira. Podemos por o comando 

  JS Jest
  reporter.addAttachment(name: string, buffer: any, type: string)
  
  Ruby
  Allure.add_attachment(name: "attachment", source: "Some string", type: Allure::ContentType::TXT, test_case: false)

  '''Não precisei fazer nenhum tipo de import no JS Jest, porque é feita a injeção quando vai executar o código.'''

  Dentro de um 'AfterAll', e automagicamente o allure ira anexar dentro da suite 

  também é possivel anexar dentro do teste alterando apenas o 'AfterAll' para um 'AfterEach' e os anexos irão para dentro do teste. 

Já que utilizar o BDD como guia de execução dos testes podemos adicionar o contéudo dentro da Feature, colocando utilizando o afterFeature que alguns frameworks dão suporte. 
Dentro do cenário usando um 'afterScenario' o anéxo ira para o final dos passos. 
E também podemos fazer um em cada step, aqui tem um detalhe da cada framework, alguns frameworks tem um 'afterStep' o que ajuda muito nesse tipo de implementação, em outros não tem esse suporte você tendo que colocar no final de cada passo(step) o código para anexar o conteúdo. 


### O que anexar ? 

Podemos anexar qualquer coisa na real, então vamos quebrar um pouuco o código. 

  JS
  reporter.addAttachment(name: string, buffer: any, type: string)

  Primeiro parametro é o nome que vai parar no anéxo. 
  No caso 'body', mas aqui pode ser qualquer string.

  Segundo é o contéudo, um texto, uma imagem, um video, um CSV entre outros. 

  O terceito é o tipo do arquivo que você esta subindo, em alguma linguagens existe um enum que facilita esse ponto, em outras é boa sorte. 

  Aqui vai uma dica de tipo de arquivos que podemos adicionar.
    '''Ruby'''
    "text/plain"
    "application/xml"
    "text/csv"
    "text/tab-separated-values"
    "text/css"
    "text/uri-list"
    "image/svg+xml"
    "image/png"
    "application/json"
    "video/webm"
    "image/jpeg"

    JS


### Exemplo de como fica. 

<p align="center">
    <img src="https://docs.qameta.io/allure/images/testcase.png" alt="Exemplo">
</p>


 Exemplo de report completo do Allure
 https://demo.qameta.io/allure/#suites/1bbdddb8b139ac506f125d271028f72a/ca0bdae7438b29d/


 Link do projeto para o Jest que não é suportado oficialmente
 https://github.com/zaqqaz/jest-allure


 ## Beneficios 
  
  Ter mais informações que já são geradas nos testes. 
  Centralizar a informação em um unico lugar

 ## Cuidados

  Você literalmente pode armazenar quanta informação quiser e isso pode fazer com que você tenha problemas com espaço, dependendo de onde o seu relatório é geraodo. 

