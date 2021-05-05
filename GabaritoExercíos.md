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