## Порядок создания хранилища

### Actions

Создаем папку `actions` и в ней файлы:
 - `action-types.js`, в котором определены все возможные типы действий
 - `JS` файлы для функций-создателей действий (`ActionCreator`), сгруппированных по разделам хранилища (например, `user-actions.js`, `widget-actions.js`, ets.)
 
`action-types.js`
```
// Users
export const GET_USERS_REQUEST = 'GET_USERS_REQUEST';
export const GET_USERS_SUCCESS = 'GET_USERS_SUCCESS';
export const USER_PROFILE_REQUEST = 'USER_PROFILE_REQUEST';
export const USER_PROFILE_SUCCESS = 'USER_PROFILE_SUCCESS';

// Widgets
export const GET_WIDGETS_SUCCESS = 'GET_WIDGETS_SUCCESS';
export const GET_WIDGETS_REQUEST = 'GET_WIDGETS_REQUEST';
```

`user-action.js`
```
import * as types from './action-types';

export function getUsersRequest() {
    return {
        type: types.GET_USERS_REQUEST
    };
}

export function getUsersSuccess(users) {
    return {
        type: types.GET_USERS_SUCCESS,
        payload: users
    };
}

export function getUserProfileRequest(userId) {
    return {
        type: types.GET_USER_PROFILE_REQUEST,
        payload: userId
    };
}

export function getUserProfileSuccess(userProfile) {
    return {
        type: types.GET_USER_PROFILE_SUCCESS,
        payload: userProfile
    };
}
```

`widget-action.js`
```
import * as types from './action-types';

export function getWidgetsRequest() {
    return {
        type: types.GET_WIDGETS_REQUEST
    };
}

export function getWidgetsSuccess(users) {
    return {
        type: types.GET_WIDGETS_SUCCESS,
        payload: {users: users}
    };
}
```


### Redusers

Создаем папку `redusers`, в которой будут находится все файлы редукторов, а также файл `redusers.js`, в котором эти редукторы собираются

`userReduser.js`
```
import * as types from '../actions/action-types';
import _ from 'lodash';

const initialState = {
    users: [],
    userProfile: {}
};

const userReducer = function(state = initialState, action) {
    switch(action.type) {
        case types.GET_USERS_SUCCESS:
            return {...state, users: action.payload);
        case types.GET_USER_PROFILE_SUCCESS:
            return {...state, userProfile: action.payload);
        // ...             
    }
    return state;
};

export default userReducer;
```

`redusers.js`
```
import { combineReducers } from 'redux';

import userReducer from './userReducer';
import widgetReducer from './widgetReducer';

export default combineReducers({
    userState: userReducer
    widgetState: widgetReducer,
});
```

### Store
 
На основе скомбинированных редукторов создаем хранилище
 
`store.js`
```
import { createStore } from 'redux';
import reducers from './reducers/redusers';

export default createStore(reducers);
```




 
 
