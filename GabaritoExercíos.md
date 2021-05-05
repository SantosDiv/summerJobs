# Gabarito dos exercícios
## Exercício 1 - O restaurante
### Store
```js
import { createStore, combineReducers } from 'redux';
import { composeWithDevTools } from 'redux-devtools-extension';
import rootReducer from '../reducers';

const store = createStore(combineReducers(rootReducer), composeWithDevTools());

export default store;
```

### Reducer
- `requestReducer.js`
```js
const INITIAL_STATE = {
  requests: [],
};

const requestsReducer = (state = INITIAL_STATE, action) => {
  switch (action.type) {
  case 'ADD':
    return {
      ...state,
      requests: action.requests,
    }
  default:
    return state;
  }
}

export default requestsReducer;
```
- `rootReducer.js`
```js
import reducerRequests from './reducerRequests';

const rootReducer = {
  reducerRequests,
}

export default rootReducer;
```

### Action
```js
const requestsAction = (requests) => ({
  type: 'ADD',
  requests,
});

export default requestsAction;
```

### Caixa.js - Componente
```js
import React from 'react';
import { connect } from 'react-redux';
import requestsAction from '../exercice_summer/redux/actions/requestsAction'

class Caixa extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      pedidos: [],
    };

    this.handleClick = this.handleClick.bind(this);
  }

  handleClick({ target }) {
    this.setState((oldState) => ({
      pedidos: [...oldState.pedidos, target.innerText],
    }));
  }

  render() {
    const { dispatchRequests } = this.props;
    return (
      <div>
        <button type="button" onClick={ this.handleClick } >
          Filé com queijo
        </button>
        <button type="button" onClick={ this.handleClick }>
          Feijão tropeiro
        </button>
        <button type="button" onClick={ this.handleClick }>
          Arroz de leite com carne de sol
        </button>
        <button type="button" onClick={ () => dispatchRequests(this.state) }>
          Dispachar Pedido
        </button>
      </div>
    );
  }
}

const mapDispatchToProps = (dispatch) => ({
  dispatchRequests: (requests) => dispatch(requestsAction(requests)),
});

export default connect(null, mapDispatchToProps)(Caixa);
```

### Cozinha.js - Componente
```js
import React from 'react';
import { connect } from 'react-redux';

class Cozinha extends React.Component {
  render() {
    const { requests } = this.props;

    return (
      <div>
        <h1>Pedidos Atuais - Cozinha</h1>
        <ul>
          { requests.map(({ food }) => <li key={ food }>{food}</li>) }
        </ul>
      </div>
    );
  }
}

const mapStateToProps = (state) => ({
  requests: state.reducerRequests.requests,
});

export default connect(mapStateToProps)(Cozinha);
```

## Exercício Bônus
### Store
**A Store permanece igual ao exercício anterior**

### Reducer
- `requestsReducer.js`
```js
const INITIAL_STATE = {
  requests: [],
  totalPrice: 0,
};

const requestsReducer = (state = INITIAL_STATE, action) => {
  switch (action.type) {
  case 'ADD':
    return {
      ...state,
      requests: action.requests,
      totalPrice: action.totalPrice,
    }
  default:
    return state;
  }
}

export default requestsReducer;
```
- `rootReducer.js` - Permanece igual ao exercício anterior;

### Action
```js
const requestsAction = (requests, totalPrice) => ({
  type: 'ADD',
  requests,
  totalPrice,
});

export default requestsAction;

```

### Caixa.js
```js
import React from 'react';
import { connect } from 'react-redux';
import requestsAction from '../exercice_summer/redux/actions/requestsAction'

class Caixa extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      requests: [],
      totalPrice: 0,
    };

    this.handleClick = this.handleClick.bind(this);
  }

  handleClick(target, price) {
    this.setState((oldState) => ({
      requests: [...oldState.requests, { food: target.innerText, price }],
      totalPrice: oldState.totalPrice + price,
    }));
  }

  render() {
    const { dispatchRequests } = this.props;
    const { requests, totalPrice } = this.state;
    return (
      <div>
        <div>
          <h3>Tabela de preços</h3>
          <p>Filé com queijo - R$ 40,00</p>
          <p>Feijão tropeiro - R$ 25,00</p>
          <p>Arroz de leite com carne de sol - R$ 20,00</p>
        </div>
        <button
          type="button"
          onClick={ ({ target }) => this.handleClick(target, 40) }
        >
          Filé com queijo
        </button>
        <button
          type="button"
          onClick={ ({ target }) => this.handleClick(target, 25) }
        >
          Feijão tropeiro
        </button>
        <button
          type="button"
          onClick={ ({ target }) => this.handleClick(target, 20) }
        >
          Arroz de leite com carne de sol
        </button>
        <button
          type="button"
          onClick={ () => dispatchRequests(requests, totalPrice) }>
          Dispachar Pedido
        </button>
      </div>
    );
  }
}

const mapDispatchToProps = (dispatch) => ({
  dispatchRequests: (requests, totalPrice) => dispatch(
    requestsAction(requests, totalPrice)),
});

export default connect(null, mapDispatchToProps)(Caixa);
```
### NotaFiscal.js
```js
import React from 'react';
import { connect } from 'react-redux';

class NotaFiscal extends React.Component {
  render() {
    const { requests, totalPrice } = this.props;

    return (
      <div>
        <h1>Nota Fiscal -- Restaurante DellyCious</h1>
        { requests.map(({ food, price }) =>
          <p key={ food }>
            {`${food} - R$ ${price},00`}
          </p>) }
        <p>{`Total --- R$ ${totalPrice},00`}</p>
      </div>
    );
  }
}

const mapStateToProps = (state) => ({
  requests: state.reducerRequests.requests,
  totalPrice: state.reducerRequests.totalPrice,
});

export default connect(mapStateToProps)(NotaFiscal);
```

## App.js
```js
import React from 'react';
import Caixa from './components/exercice_summer/Caixa';
import Cozinha from './components/exercice_summer/Cozinha';
import NotaFiscal from './components/exercice_summer/NotaFiscal';

class App extends React.Component {
  render() {
    return (
      <div className="App">
        <Caixa />
        <Cozinha />
        <NotaFiscal /> // Componente do exercício Bônus
      </div>
    );
  }
}

export default App;
```

## Index.js
```js
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import store from './components/exercice_summer/redux/store/';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';

ReactDOM.render(
  <Provider store={ store }>
    <App />
  </Provider>,
  document.getElementById('root')
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();

```