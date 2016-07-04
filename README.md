# redux-ru-tutorial
Код для [Создание actions](https://maxfarseer.gitbooks.io/redux-course-ru/content/sozdanie_actions.html)

## Ключевые моменты

### Store

Одно хранилище на все приложение.

Представляет собой многоуровневый объект, объединяющий в себя данные приложения

### Actions

Простой объект, созданный на основе [соглашения](https://github.com/acdlite/flux-standard-action)

```
{
  type: 'ACTION_TYPE',
  payload: {
    // some data  
  }
}
```

Для описания типов действий используются константы:

```
const SOME_ACTION = 'SOME_ACTION';
const ANOTHER_ACTION = 'ANOTHER_ACTION';
```

Для вызова действия используется функция `ActionsCreator`, которая принимает `payload` и возвращает `action`:

```
const someAction = payload => ({
    type: SOME_ACTION,
    payload
});

/* 
Здесь нет синтаксической ошибки. Функция реально возвратит объект
{
    type: SOME_ACTION,
    payload: payload // здесь значение свойства payload - это аргумент функции
}   
*/
```

### Redusers

На основании действий (`actions`) редукторы (`redusers`) изменяют состояние глобального хранилища (`store`)

Глобальное хранилище делится на функциональные части, за каждую из которых отвечает свой редуктор:

```
store = {
    clients: {
    // data for part "clients"
    },
    claims: {
    // darta for part "claims"
    },
    ...
}
```
*Примечание: В приложении не нужно будет писать подобный код. Это лишь описание структры хранилища*

Редуктор принимает `action` и возвращает **новое** состояние хранилища. (Именно новое состояние, а не измененное старое!)

```
const initialState = {
    clients: []
};

const clientsReduser = function (state = initialState, action) {
    switch (action.type) {
        case GET_USER_LIST:
            return { ...state, clients: action.payload };  //Object spread syntax
        default:
            return state
    }
}
```
*Примечание: Для создания новго состояния хранилища используем [Object spread syntax](https://github.com/sebmarkbage/ecmascript-rest-spread)*

Перед созданием хранилища редукторы комбинируются с помощью `Redux` функции `combineReducers()`:

```
import { combineReducers } from 'redux';

const redusers =  combineReducers({
    clients: clientsReducer,
    claims: claimsReducer
});
```
*Примечание: Свойства объекта, который передается функции комбинирования соответсвуют функциональным частям хранилища* 