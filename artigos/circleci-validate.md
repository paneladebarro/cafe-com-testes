<p align="center">
  <a href="https://github.com/pagarme/cafe-com-testes">
    <img src="../.github/cafecomtestes.png" alt="Café com Testes">
  </a>
</p>

# Validando as alterações no CircleCI antes do push

O [CircleCI](https://circleci.com/) é uma ferramenta de Integração Contínua as a service e muito utilizada. Toda configuração é feita utilizando um arquivo yml como no exemplo abaixo:

```yml
version: 2.1
jobs:
  build:
    docker:
      - image: alpine:3.7
    steps:
      - run:
          name: The First Step
          command: |
            echo 'Hello World!'
            echo 'This is the delivery pipeline'
```

Um problema que acontece bastante quando mudamos algum item da configuração, é acabar não prestando atenção na hierarquia dos componentes e só depois de fazer o push para o repositório e começar a execução do pipeline é que vemos esses problemas através de um erro de build.

<p align="left">
  <a href="https://github.com/pagarme/cafe-com-testes">
    <img src="../.github/circleci-error.png" width="300" alt="Café com Testes">
  </a>
</p>

Para evitar isso e já antecipar possíveis erros de configuração, você pode utilizar o CircleCI Validate. 

Para instalar em Linux basta:

```shell
sudo snap install docker circleci
sudo snap connect circleci:docker docker
```

Para instalar em outros sistemas operacionais você pode acessar a [documentação oficial](https://circleci.com/docs/2.0/local-cli/).

Depois de instalado basta executar:

```shell
circleci config validate
```

Se tudo estiver bem, você vai receber a seguinte mensagem:

```shell
Config file at .circleci/config.yml is valid.
```

E se tiver qualquer problema relacionado a configuração, você receberá uma mensagem indicando que existe um problema e onde ele está.

```shell
Error: Unable to parse YAML
mapping values are not allowed here
 in 'string', line 9, column 18:
              command: |
                     ^
```

Assim você consegue validar antes mesmo de subir suas alterações. Para facilitar no processo de desenvolvimento, uma dica é adicionar essa validação utilizando uma biblioteca de hooks como o [husky](https://github.com/typicode/husky) para sempre executar essa validação antes do seu commit e/ou push.
