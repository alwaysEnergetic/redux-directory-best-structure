# redux directory structure

This is the one of the coding styles for react redux. 

## Installation

Use the package manager [npm](https://www.npmjs.com/) to install and start.

```bash
npm install

npm start
```

## Structure

    .
    ├── ...
    ├── state                       # Redux Root Folder 
    │   ├── ducks
    │       ├── ...
    │       ├── channels            # One of the bunches of reducer, action and etc
    │           ├── actions.js      # Action 
    │           ├── index.js        # Channels index
    │           ├── reducers.js     # Reducer
    │           ├── types.js        # Type
    │           ├── utils.js        # Utils
    │       ├── index.js            # Ducks index
    │       ├── ...
    │   ├── utils
    │       ├── createReducer.js
    │       ├── fetch.js
    │       ├── index.js
    │       ├── normalizrMiddleware.js
    │       ├── selectors.js
    │   ├── schemas.js              # Schemas
    │   ├── store.js                # Store
    │   └── ...                     # etc.
    └── ...

## Inside the files

### store.js

``` javascript
    import { createStore, applyMiddleware, combineReducers } from "redux";
    import promiseMiddleware from "redux-promise-middleware";
    import normalizrMiddleware from "./utils/normalizrMiddleware";
    import thunkMiddleware from "redux-thunk";
    import * as reducers from "./ducks";
    import { LOG_OUT } from "./ducks/user/types";

    const rootReducer = (state, action) => {
      if (action.type === LOG_OUT) state = undefined;
      return combineReducers(reducers)(state, action);
    };

    const reduxDevTools =
      process.env.NODE_ENV === "development"
        ? window.__REDUX_DEVTOOLS_EXTENSION__ &&
          window.__REDUX_DEVTOOLS_EXTENSION__()
        : window.__REDUX_DEVTOOLS_EXTENSION__ &&
          window.__REDUX_DEVTOOLS_EXTENSION__(); // before production {}

    export default createStore(
      rootReducer,
      reduxDevTools,
      applyMiddleware(thunkMiddleware, promiseMiddleware(), normalizrMiddleware())
    );
```
### schemas.js

``` javascript
    import { schema } from "normalizr";

    export const specialist = new schema.Entity("specialists");

    export const channel = new schema.Entity("channels", {
      specialists: [specialist]
    });

    export const channels = new schema.Entity(
      "channels",
      {},
      {
        processStrategy: entity => {
          entity.specialists.reverse();
          return entity;
        }
      }
    );

    export const team = new schema.Entity("teams", {
      channels: [channel]
    });

    export const teams = new schema.Entity("teams", {
      teams: [team]
    });

    export const epic = new schema.Entity("epics");

    export const project = new schema.Entity("projects", {});

    export const task = new schema.Entity("tasks", {});
```

### ducks/index.js

``` javascript 
    export { reducer as form } from "redux-form";

    export { default as user } from "./user";
    export { default as profile } from "./profile";

    export { default as projects } from "./projects";
    export { default as epics } from "./epics";
    export { default as tasks } from "./tasks";

    export { default as teams } from "./teams";
    export { default as channels } from "./channels";

    export { default as specialists } from "./specialists";

    export { default as search } from "./search";

    export { default as skills } from "./skills";
    export { default as experienceLevels } from "./experienceLevels";
    export { default as industriesReducer } from "./industries";
    export { default as projectTypesReducer } from "./projectTypes";

    export { default as modals } from "./modals";
    export { default as sidebar } from "./sidebar";
    export { default as kanban } from "./kanban";
```

## Enjoy!!! 🎇
