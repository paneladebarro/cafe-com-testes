<p align="center">
  <a href="https://github.com/pagarme/cafe-com-testes">
    <img src="../.github/cafecomtestes.png" alt="Café com Testes">
  </a>
</p>

# Try/Catch: deixe os testes lidarem com as exceções

É comum você se deparar por aí com um try/catch nos testes automatizados. As pessoas costumam utilizar isso para garantir o lançamento de exceções caso aconteça algum problema na execução do teste, porém isso é um code smell que além de dificultar a leitura dos testes (eles deixam de ser DAMP) também adiciona complexidade desnecessária nos mesmos. Inclusive é uma regra que o [Sonar valida](https://rules.sonarsource.com/java/RSPEC-3658), o certo é deixar o teste lidar com as exceções e se utilizar dos recursos do próprio framework para fazer essas validações. 

No exemplo abaixo estamos utilizando [Jest](https://jestjs.io/docs/pt-BR/expect) sobre como testar o lançamento de erros.

## Exemplo
```javascript
it('should NOT throw error if name is given', () => {
  const expectedToThrow = () => {
    validateUser({ name: 'my name' }, validateUserName())
  }
  // a expectativa é que não seja lançado nenhum erro
  expect(expectedToThrow).not.toThrow()
})
it('should throw error of invalid user name if it is null', () => {
  const expectedToThrow = () => {
    validateUser({ name: null }, validateUserName())
  }
  // a expectativa é que seja lançado o erro de invalid user name
  expect(expectedToThrow).toThrow(new Error('invalid user name'))
})
```

Outro exemplo utilizando [ava](https://github.com/avajs/ava)

```javascript
await t.throwsAsync( async () => {
    const error = await validateUser({ name: null }, validateUserName())
    // a expectativa é que seja lançado o erro de invalid user name
    t.is(error.name, 'ValidationError')
  }, { message: `invalid user name`})
```

Então sempre que possível procure utilizar os recursos do próprio framework de testes para lidar com o lançamento de exceções ao invés de utilizar try/catch.
