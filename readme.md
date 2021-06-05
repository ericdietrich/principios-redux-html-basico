# Redux

Redux é um padrão e uma biblioteca que gerencia e atualiza estados de uma aplicação, usando eventos chamados "actions". Provê um store centralizado de estados que serão utilizados por toda a aplicação.

Seu uso demanda conhecimento de novos conceitos e mais código escrito, além de exigir que certas restrições sejam seguidas. Portanto, não é todo projeto que se beneficia de suas vantagens. Seu uso é indicado quando:

- a aplicação possui grande quantidade de estados utilizados em vários locais
- os estados são frequentemente atualizados
- a lógica de atualização dos estados é complexa
- o código da aplicação é médio ou grande e é utilizado por muitos desenvolvedores

![https://miro.medium.com/max/700/1*BcmtHcMHN6PT7IniIWniHg.png](https://miro.medium.com/max/700/1*BcmtHcMHN6PT7IniIWniHg.png)

![https://redux.js.org/assets/images/ReduxDataFlowDiagram-49fa8c3968371d9ef6f2a1486bd40a26.gif](https://redux.js.org/assets/images/ReduxDataFlowDiagram-49fa8c3968371d9ef6f2a1486bd40a26.gif)

## Conceitos

### Store

É o centro da aplicação Redux. O store é um container onde ficam os estados globais da aplicação.

**Store**: é o container que armazena e centraliza o estado geral da aplicação. Ela é imutável, ou seja, nunca se altera, apenas evolui.

> Podemos ter apenas uma Store por aplicação, ou seja, ela é a Única Fonte de Verdade (Single Source of Truth).

Se apresenta como uma objeto javascript com funções e habilidades:

- o estado mantido dentro do store nunca deve ser modificado diretamente
- a atualização do estado deve ser feita por um objeto de ação ("action") que descreve algo que aconteceu na aplicação, e que então expede ("dispatch") a ação para que o store atualize o estado
- a atualização do estado no store ocorre quando este executa a função **reducer** e calcula o novo estado baseado no antigo estado.
- por último, o store notifica a atualização para que a interface atualize o dado alterado

```jsx
// Create a new Redux store with the `createStore` function,
// and use the `counterReducer` for the update logic
const store = Redux.createStore(counterReducer);
```

### Actions

São fontes de informações que são enviadas da aplicação para a Store. São disparadas pelas Action Creators, que são simples funções que, ao serem executadas, ativam os Reducers.

```jsx
document.getElementById("increment").addEventListener("click", function () {
  store.dispatch({ type: "counter/incremented" });
});
```

A action, nesse caso é objeto:

`{ type: "counter/incremented" }`

Esse objeto sempre possui um campo `type`, que é uma string dá nomeia de forma única a ação. Devem ter nomes que descrevem com clareza sua função, como por exemplo "counter/incremented". A primeira parte descreve o tipo da ação e a segunda parte descreve o que aconteceu.

### Reducer

Recebem e tratam as informações para que sejam ou não enviadas à Store. O reducer recebe dois argumentos: o `state` atual e uma `action`

```jsx
// Create a "reducer" function that determines what the new state
// should be when something happens in the app
function counterReducer(state = initialState, action) {
  // Reducers usually look at the type of action that happened
  // to decide how to update the state
  switch (action.type) {
    case "counter/incremented":
      return { ...state, value: state.value + 1 };
    case "counter/decremented":
      return { ...state, value: state.value - 1 };
    default:
      // If the reducer doesn't care about this action type,
      // return the existing state unchanged
      return state;
  }
}
```

### UI

A interface do usuário vai a apresentar o estado conforme este se apresentar. Precisando ser renderizada no carregamento e a cada alteração

```jsx
// Our "user interface" is some text in a single HTML element
const valueEl = document.getElementById("value");

// Whenever the store state changes, update the UI by
// reading the latest store state and showing new data
function render() {
  const state = store.getState();
  valueEl.innerHTML = state.value.toString();
}

// Update the UI with the initial data
render();
// And subscribe to redraw whenever the data changes in the future
store.subscribe(render);
```
