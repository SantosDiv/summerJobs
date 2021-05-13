# Redux com React
## O que vamos aprender?

Seja bem-vindo a mais um conteúdo do módulo front-end. Aqui, você irá aprender a desenvolver e gerenciar o **estado global** da sua aplicação React, utilizando o **Redux**. Resolvendo assim, alguns problemas de fluxo da sua aplicação, para então deixar o seu código ainda mais entendível e limpo.

## Você será capaz de:

 - Criar estado global com o **Redux** -> (Store)
 - Integrar este estado com o React -> (connect, Provider)
 - Gerenciar o estado com o **React-Redux** -> (Reducers, Actions)

## Porque isso é importante?

Imagine que você tem uma aplicação grande, onde muitos componentes precisam de informações uns dos outros, mesmo sem esses serem parentes diretos (pai - filho - neto). Aprendemos que para passar tais informações utilizamos as `props`, passando `callbacks`, e outros dados, para então provermos estas informações a toda a nossa aplicação.

A maioria dos *Frameworks*, possuem seus proprios gerenciadores de estado global, sem precisar de uma biblioteca externa. No React, por exemplo, é o `context` . Mas, porque devo usar o *Redux*? A resposta é: porque para uma aplicação grande, os gerenciadores de estados nativos podem trazer mais complexidade que o normal, então entramos com o Redux para resolver esse problema e deixar o nosso código menos sujeito a erros e mais nítido.

O Redux veio 'curar' a dor que os desenvolvedores passavam quando se tratava de passar/usar informações pela aplicação. Um deles é o que chamamos de `prop drilling`, ou seja, em uma tradução livre, a escavação de props. Na qual, você tem que pegar uma informação que está em um componente lá embaixo da árvore de componentes e então leva-la para onde você deseja.

Assim, entendendo a dor que ele veio 'curar' e o porque usar com o React, vamos colocar a mão na massa :D.


## Conteúdo

Bem alimentados com o conceito do porque estamos aqui, vamos entrar no Redux junto ao React e algumas das peças que os compoẽm, e assim irmos montando toda a nossa aplicação, com um estado global feliz e completo.

Para ilustramos o conceito do redux unido ao React, vamos imaginar que você  está indo ao Shopping com algumas sacolas e precisará usar o **guarda volume** deles. Lá vai ter um/uma **atendente** que vai te dar algumas instruções de como usar aquele espaço. Vamos lá?

### Instalando as bibliotecas
Antes de iniciarmos a explicação, é importante instalarmos as bibliotecas que nos auxiliarão na criação do nosso estado global. No terminal da sua aplicação digite o seguinte comando:
```sh
  npm i redux react-redux --save
```
Agora com tudo instalado, podemos continuar :D

### Store - O armário
Você acabou de chegar ao shopping e espera que nele tenha um ótimo espaço, organizado, limpo e o principal, de fácil acesso, para você poder guardar suas sacolas de forma segura e que não se percam.

Você vê o local, e nele tem um grande **armário**, que é dividido em vários pequenos quadrados, tudo muito organizado, com tranca em cada 'quadradinho', onde você pode colocar as suas sacolas.

Para a nossa sorte, um desenvolvedor já havia criado este espaço para nos receber. Este espaço em *Redux* se chama **Store**. Ele nada mais é que o local (*armário*) onde ficará guardado todos os dados (*sacolas*) que enviaremos (*guardaremos*), para que, quando você precisar, possa ter fácil acesso e pega-los de volta. Veja como ele fez:
```javascript
// Importando as funções e arquivos necessários
import { createStore, combineReducers }  from 'redux';
import rootReducer from '../reducers';
// Criando o store
const store = createStore(combineReducers(rootReducer));
// exportando o store
export default store;

```
A função `createStore`, de forma intuitiva, cria a nossa *Store*, cria o armário, da nossa aplicação. Nós a guardamos em uma variável chamada `store`, pois iremos precisar dela em outro arquivo da nossa aplicação.

Mas o que significa o `combineReducers` e o `rootReducer`? Iremos entrar neles em seguida :D.

*Dica Show: Para visualizar o seu estado global, você pode utilizar uma biblioteca chamada `redux-devtools-extension`, basta instala-la com o `npm install redux-devtools-extension` e adicionar ela a sua store. Observe:*
```javascript
import { createStore, combineReducers } from 'redux';
import { composeWithDevTools } from 'redux-devtools-extension'; // EXTENSÃO
import rootReducer from '../reducers';

const store = createStore(combineReducers(rootReducer), composeWithDevTools()); // CREATE RECEBENDO A EXTENÇÃO

export default store;

```
*Agora, vá ao seu chrome e instale a extensão: `Redux DevTools`. Prontinho! Assim você vai conseguir acompanhar a sua store, enquanto você desenvolve.*

- Exercício de fixação:
**Ajude o Ash: Onde ele está errando na criação da store? E depois salve o arquivo correto numa pasta chamada: store**
```javascript
import { combineReducers } from 'react-redux';
import createStore from '../reducers';

const store = createStore(combineReducers());

export default sotre;
```


### Reducer - A pessoa atendente
Continuando o seu desejo de guardar suas sacolas, você vai até o local do guarda volume, onde lá é atendido por uma pessoa muito educada e organizada. Essa pessoa vai te dar algumas opções em um tablet, sobre o que você deseja fazer, para assim guardar/retirar as suas, sem misturar com as demais.

Essa pessoa atendente é o que podemos chamar de **reducer** no contexto do Redux. Ele é uma função que **nós** criamos, onde vai determinar como vai ser o estado inicial de um dos espaços da nossa *store* e que irá controlar como vamos alterar este estado.

Ou seja, assim como no Shopping, onde você só consegue adicionar ou retirar algo do guarda volume com o intermédio da pessoa atendente, você também só poderá alterar o seu estado global (store) através do **reducer**. Sem ele, é como se estivesse fechado o guarda-volumes e não tem acesso ao seu *armário*.
Mas como fazemos isso em React? Veja:
```javascript
// Estado inicial
const INITIAL_STATE = {
  squares: [],
};
  // Reducer
  const luggageStorageReducer = (state = INITIAL_STATE, action) => {
    switch (action.type) {
      case 'ADD': // Condição de adicionar sacolas
        return {
        ...state,
        squares: [...state.squares, action.item],
      };
      case 'REMOVE': // Condição de remover sacolas
      return {
        ...state,
        squares: action.item,
      };
          default:
            return state;
    }
  };
```
Entendendo o código:
Nós iniciamos o estado do nosso armário, onde vai ficar todas as sacolas de todas as pessoas que chegarem no shopping e quiserem adicionar elas lá. Chamamos essa variável de `INITIAL_STATE`.

O **reducer** recebe dois argumentos, o primeiro é o estado (state) inicial, que é um objeto com uma chave que dei o nome de `squares`. O segundo argumento é a **action**, este é o comando do que queremos fazer. A pessoa atendente (voltando no exemplo do shopping) só entende esses dois comandos, por enquanto, `add` e `remove`. Só sabe adicionar novas sacolas, ou retirar elas.

Dentro dessa função tem um `switch`, ele serve para ser esse filtro, que diz o que deverá ser feito ou não, a partir do que será passado pela `action` (vamos entrar nela daqui a pouco).

- Exercício de Fixação:
**Crie um reducer, onde o estado inicial é um objeto que tem uma chave chamada, `toggle: false`. E que tem apenas uma `action` no switch que pode mudar para `true` ou `false` o valor da chave `toggle`**;

### rootReducer e CombineReducer()
Você deve está lembrado do combineReducers e do rootReducer, quando colocamos ele lá na criação da nossa Store.

O `rootReducer` nada mais que um arquivo com vários reducers juntos. Mas para que criar um arquivo? Bom, a nossa *Store* pode receber apenas **um objeto**. Por isso, usamos o `rootReducer` (Que pode ser qualquer nome), para exportar mais de um reducer e um único objeto e a nossa store possa ter vários objetos nele.

Para fazer essa combinação usamos o `combineReducers()` passando como parâmetro o `rootReducer`. Veja:

- criando o rootReducer:
```javascript
import reducer1 from '../reducer/reducer1';
import reducer2 from '../reducer/reducer2';
...
const rootReducer = {
  reducer1,
  reducer2,
  ...
};
export default rootReducer;
```
Depois basta colocar o rootReducer dentro do combineReducers():
`createStore(combineReducers(rootReducer))`.

**Importante**: O reducer precisa ser uma função pura, ou seja, que retorna algo e não faz operações antes do return. Logo evite fazer operações, condicionais (a não ser o switch) de forma que perca o propósito do reducer. [Veja aqui mais sobre funções puras](https://medium.com/devzera/o-que-%C3%A9-uma-fun%C3%A7%C3%A3o-pura-em-javascript-2b34edcad8e2)

- Exercício de fixação:
**Crie mais um reducer, agora que tem um estado inicial que é um objeto que tem uma chave que é uma string vazia chamada `name`. Ele tem duas actions, uma para colocar o nome, e outra para voltar a chave para o estado inicial. Após ter feito esse reducer, una com o reducer feito anteriormente em um arquivo chamado `rootReducer`. Por fim coloque importe esse arquivo na store que você criou mais acima, e então coloque-o no lugar correto.**.


### Actions - Seus pedidos a pessoa atendente

Finalmente você pode pedir para guardar suas sacolas. Para isso, a pessoa atendente (Reducer) te fará a seguinte pergunta: *Olá, bem-vindo! Você quer guardar, retirar ou saber se tem espaço disponível?*. Cada uma dessas opções vai causar uma ação diferente concorda? Dependendo do que você escolha. Pois, bem! Seguindo esse pensa meto, as **actions** no Redux são funções que mandam as ordens que você manda para o Reducer, e dependendo dessa ordem, será feita uma coisa diferente no estado.

Escrevendo uma action para adicionar um novo item a Store
```javascript
  const addStorage = (item) => ({
    type: 'ADD',
    item,
  });
```
Veja que a action é uma função que pode ou não receber parâmentros, mas que deve sempre retornar um **objeto**, contendo sempre uma chave chamada `type` e, caso receba algum elemento como parâmetro, contendo esse elemento.

Essa chave `type` (que tem sempre que ter esse nome) é a **Ordem** que você está dando. Neste caso, é a ordem de adicionar. O reducer vai entender isso, e entrar na primeira condição do switch e adicionar ao array um novo item. :D

Muita coisa né? Tudo bem, nós criamos as nossas peças. Vamos agora interligar tudo isso e ficará mais nítido.

- Exercício de Fixação: Faça às duas `actions` (em dois arquivos diferentes), para os dois reducers que você criou anteriormente. Respeitando o propósito de cada Reducer.

### Provider - A porta de entrada


Nós criamos o nosso arquivo `store` e exportamos ele. Agora a nossa aplicação precisa poder usar ele, por isso vamos utilizar um componente do `React-redux` que nos possibilita utilizar o store criado, na nossa aplicação.

Vá até o index da sua aplicação e importe esse módulo e também importe o seu store. Veja:
```jsx
  ...
  import { Provider } from 'react-redux'; // Componente Provider
  import store from '../store'; // Store que criamos
  import App from './App';
  import './App.css';

  ReactDOM.render(
   <Provider store={ store }> // Passando como prop a nossa store
     <App />
   </Provider>,
   document.getElementById('root'),
  );
```

- Exercício de Fixação: **Faça a importação do componente `Provider` e da `store` para aplicação que você criou, para o local correto.**

### mapDispatchToProps() - A ordem de envio

Lembra que a pessoa atendente te deu três opções? Guardar, retirar ou saber o espaço? Pois, bem! Chegou a hora de responder a essa pessoa.

A forma de responder é com o **mapDispatchToProps**, ou seja, você vai realmente dispachar uma ação para o seu reducer, ativando a sua action e então alterando o seu estado. Legal né? Estamos começando a conectar as peças que criamos pouco tempo atrás.

Ele é uma função que retorna um objeto que, retorna uma nova função. Confuso? Calma, veja abaixo a declaração:

```javascript
  const mapDispatchToProps = (dispatch) => ({
    add: (item) => dispatch(addStorage(item));
  });
```
Entendendo o código acima:
- `dispatch` -> Função do próprio Redux, que faz essa coneção com a nossa ação e com o nosso store.
- a chave `add` ⇾ Ela é o nome que eu escolhi para que eu posso colocar no meu documento e executar essa função;
- `addStorage` -> Action que criamos na seção anterior.

Veja que ela recebe um parâmetro que chamamos de `item` que é o item que queremos guardar no nosso store.

**Usando a função add()**
```javascript
import React from 'react';
import { addStorage } from '../actions';

class StorageLaunch extends React.Component {
  render() {
    const { add } = this.props;
    const item = { id: 0, object: 'sacolas', persona: 'Marcos' }
    return (
      <div>
        <h1>Bem vindo! O que deseja fazer? Guardar, Retirar ou saber o estoque?</h1>

        <button type="button" onClick={() => add(item)}>Guardar</button>
      </div>
    );
  }

}

const mapDispatchToProps = (dispatch) => ({
  add: (item) => dispatch(addStorage(item));
});

export default connect(null, mapDispatchToProps)(StorageLaunch);
```
Veja que a função `add` é recebida como props, de forma 'Mágica' o Redux faz isso para nós e são passados para os nossos componentes como props.

O botão está ativando a função `add` e está passando o `item` que é um objeto, como parâmetro. Ao passo que, ele será enviado pelo mapDispatchToProps, que ativará a Action que criamos e importamos, e então mudará no nosso estado global.

- Exercício de Fixação: **Crie um componente chamado `ShowName` nele crie um input, e três botões:  `Add name, Clear Name, Show/Hide` Use o `mapDispatchToProps` para colocar o nome, digitado no input, no estado global, ao clicar no botão `Add name` E poder fazer o estado voltar ao valor inicial ao clicar no botão `Clear Name`**

### connect - O que une

Agora que temos o nosso dispatchToProps, temos o nosso Provider no lugar certo, temos a nossa store, precisamos unir tudo isso, ou seja, conectar tudo isso. E para isso vamos usar uma função do `react-redux` que faz isso para nós. Sua sintaxe é a seguinte:

```javascript
  export default connect(mapStateToProps, mapDispatchToProps)(NomeDoComponente);
```
Na linha em que exportamos o nosso componente, colocamos essa função que recebe dois parâmetros: O `mapStateToProps` (Que veremos em seguida) e o `mapDispatchToProps` que acabamos de entender :D. E em seguida ela já retorna uma função que recebe como parâmetro o nome do seu componente.

**Atenção!** Lembre-se de importar a função connect do `react-redux`;

- Exercício de Fixação: **Adicione o Connect ao seu componente `ShowName` passando os parâmetros corretos para ele**.

### mapStateToProps - A ordem de recebimento
O mapStateToProps é o contrário que o DispatchToProps, agora você está solicitando ler o seu estado global. Você quer saber o que tem lá dentro e usar alguma informação que ta lá.

Ou seja, no exemplo do shopping, você está pedindo a pessoa atendente, para ele dizer qual o espaço disponível. Você só quer saber, ele não vai alterar em nada do **Guarda volume**.

Veja a sintaxe:
```jsx
  const mapStateToProps = (state) => ({
    squares: state.luggageStorageReducer.squares,
  });
```

Entendendo o código acima:
 - mapStateToProps => Função do redux que permite o acesso ao store;
 - state => Estado global da aplicação (Tudo que está guardado na nossa store);
 - squares: Nome da prop que eu dei, para guardar o valor que está no reducer que criamos e que contém a chave `squares`;

 Da mesma forma que o dispatch, aqui você te acesso a esses dados através de props, e então de forma simples consegue utilizar na sua aplicação. Veja um exemplo:

 ```javascript
 import React from 'react';
class Stock extends React.Component {
  render() {
    const { squares } = this.props;
    return (
      <div>
        <h1>Acabei de contar aqui e temos { 100 - squares.lenght } containers disponíveis </h1>
      </div>
    );
  }
}

const mapStateToProps = (state) => ({
  squares: state.luggageStorageReducer.squares,
});

export default connect(mapStateToProps)(Stock);
 ```

> Particularidades da função connect: Se caso você for usar apenas o mapDispatchToProps, é preciso colocar o parâmetro `null` onde seria o stateToProps: `connect(null, mapDispatchToProps)`. Mas se for usar apenas o mapStateToProps, não é preciso colocar o `mapDispatchToProps`: `connect(mapStateToProps)`.

- Exercício de Fixação: **Faça com que ao clicar no botão `Show/Hide` o nome apareça ou desapareça da tela. Caso não tenha nome cadastrado no estado global, coloque a mensagem: `Não há um nome cadastrado`. (Use o `mapStateToProps` para isso).**.

## Exercícios
### Exercício - O restaurante
Você foi chamado pelo dono do restaurante `DellyCious` para resolver um problema de comunicação de pedidos que ele vem enfrentando. Ele está precisando que os pedidos que o caixa cadastrar na tela dele, caia direto na tela do cozinheiro. Assim não terá problema de pedidos errados e todos ficarão felizes.

*Sua missão é a seguinte:* Utilizando o Redux unido ao React, criar dois componentes: `Caixa` e `Cozinha`, onde você mandará a informação, partindo do `Caixa`, para o estado global, e a `Cozinha` irá consumir essa informação cadastrada nele.

Informações importantes:
- O restaurante tem, por enquanto, 3 opções de pratos: **Filé com queijo**, **Feijão-tropeiro**, **Arroz de leite com carne de sol**. Serão essas opções que serão mostrados na tela do `Caixa` e que aparecerão na `Cozinha`.
- O componente `Caixa` é irmão do componente `Cozinha` não tem ligação direta entre eles.
- Crie um botão: **Dispachar Pedido** para que quando clicado, os pedidos feitos, sejam enviados para o estado global.

O que espera-se encontrar? **Store**, **Reducer**, **Actions**, **Provider**, **mapDispatchToProps**, **mapStateToProps**;

Dicas:
- Crie um diretório para o Redux, com uma pasta para o Store, Reducers e as Actions;
- Utilize botões para as opções de pratos;
- Utilize o estado do componente para guardar temporariamente os pedidos clicados;
- Exemplo do estado do reducer:
```javascript
{
  requests: [{ food: 'Filé com Queijo' }, ...],
}
```

### Exercício Bônus
Utilizando o que você criou no exercício anterior, faça o seguinte:
- Adicione os preços dos pratos no componente `Caixa` em uma tabela de preços;
- Crie um componente chamado `NotaFiscal`, nele você vai colocar as informações do pedido do cliente. Veja o exemplo de saída:
```javascript
'Nota Fiscal -- Restaurante DellyCious'
'Nome-do-prato - R$ 30,00'
'Nome-do-prato - R$ 12,00'
'Nome-do-prato - R$ 10,00'
'Total  ---  R$ 52,00'
```
- Utilizando ainda o Redux, adicione os pratos e seus respectivos preços no estado global e exiba essas informações no componente `NotaFiscal`

Informações adicionais:
- Os preços dos pratos são:
```javascript
  'Filé com Queijo - R$ 40,00'
  'Feijão tropeiro - R$ 25,00'
  'Arroz de Leite com carne de sol - R$ 20,00'
```
- Exemplo do estado do reducer:
```javascript
{
  requests: [{ food: 'Filé com Queijo', price: 40 }, ...],
}
```

## Recursos Adicionais - Opcional
- [Funções Puras](https://medium.com/devzera/o-que-%C3%A9-uma-fun%C3%A7%C3%A3o-pura-em-javascript-2b34edcad8e2)
- [Documentação React Redux - Getting Started](https://react-redux.js.org/introduction/getting-started)
- [Redux - Performance](https://redux.js.org/faq/performance)
