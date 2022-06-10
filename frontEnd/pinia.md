## [PINIA](https://pinia.vuejs.org/)
-----

- Pinia is a store like vuex for vue but it has a lot of extra features than vuex.It is built to be an prototype for vuex 5.
- has typescript support out-of-box
- Pinia doesn't use nested stores ,we can create new stores easily.
- `npm install pinia`
- ```typescript
    import { createPinia } from 'pinia'
    import { createApp } from 'vue'
    import App from './App.vue'


    const myApp=createApp(App)s
    myApp
    .use(createPinia())
    .mount("#xd")
    ```

## [defining a store](https://pinia.vuejs.org/core-concepts/)
```typescript
import { defineStore } from "pinia";
// useStore can be anything like useAuth,useMy .use "use" as prefix to be common.
// main is unique name.can be anything.
export const useStore=defineStore("main",{
//store content
})
```
## [state](https://pinia.vuejs.org/core-concepts/state.html ) :
- state is write in a arrow function for typing.
- we can change state items directly.
- ```typescript
   //declaring
    import { defineStore } from "pinia";

    export const useStore=defineStore("useStore",{
        state:()=>{
            return{
                username:"mehmet", // username:string="mehmet"s
                surname:"jnaskd"
            }
        }
    })
    ```
- ```javascript
   // accessing state and change data
    <script setup lang="ts">
    import { computed } from "@vue/runtime-core";
    import { useStore } from "../store";
    const store=useStore()
    const username=computed(()=>store.username)
    const surname=computed(()=>store.surname)
    // store.username=2 //Type 'number' is not assignable to type 'string'.
    store.username="ahmet" // ok
    const updateUsername=()=>store.username="deliasd"
    const resetStore=()=>store.$reset()
    </script>
    <template>
    <h1>
        {{username}}
    </h1>
    <button @click="updateUsername">update</button>
    <button @click="resetStore">reset</button>
    </template>
    ```
- resetting state : `store.$reset()`
- updating whose state values : `store.$state={username:"alex",surname:"seuza"}`
- updating multiple values: $patch . We can also use function format. `store.patch((state)=>)`
- watching state : as default when the component unmount $subscribing method also unmount.If we want to $subscribing method keep alive use `store.$subsribe((mutation,state)=>{},{ detached: true })`
    - ```typescript
            store.$subscribe((mutation,state)=>{
            console.log(mutation.type) // operation type . can be 'direct' | 'patch object' | 'patch function'
            console.log(mutation.storeId) // store id
            console.log(mutation.events) // to access old value and targets.
            console.log(state) // new state object.
        })
        ```

## [getters](https://pinia.vuejs.org/core-concepts/getters.html)
- getters are works as computed for state values.
- ```typescript
    // declaring
    import { defineStore } from "pinia";

    export const useStore=defineStore("useStore",{
    state:()=>{
    return{
    users:["mehmet","ahmet","derya"] //string[]
    }
    },
    getters:{
    firstUser:(state)=>state.users[1], // ()=>string | undefined
    // firtUser(){ // ()=>void
    //     this.users[1] // void
    // }
    userLength():number{ // ()=>number . when we use this, must be write return type.
    return this.users.length
    },
    firstAndLength(){
    return {length:this.userLength,firstUser:this.firstUser}
    },
    compareUser:({users})=>(user:string)=>users.indexOf(user)>-1
    }
    })
    ```
- ```typescript
    // using getters
    import { useStore } from "../store";

    const store=useStore()
    console.log(store.firstUser) // ahmet
    console.log(store.userLength) // 3
    console.log(store.firstAndLength) // {length: 3, firstUser: 'ahmet'}
    console.log(store.compareUser("mehmet")) // true
  ```
- for passing arguments to a getter use `(state)=>(parameter)=>{}`

## [Actions](https://pinia.vuejs.org/core-concepts/actions.html)
- pinia doesn't use mutations.We can change states directly or anywhere else.(in actions)
- actions like methods and they are simple functions.Can be async ,can has parameter i.e.
- ```typescript
    actions:{
    async getAlbums(){
    try{
    const res=await axios("https://jsonplaceholder.typicode.com/albums")
    if(res.status!==200) throw new Error("something went wrong")
    return res.data
    }catch(err){
    console.log(err)
    }
    },
    findUser(user:string){
    this.users.indexOf(user)>-1 ?
    console.log("user is exist") : 
    console.log("user is not exist")
    }}
   ```
- ```typescript
    import { useStore } from "../store";
    const store=useStore()
    const albums=ref(null)
    store.findUser("ahmetim") // user is exist  
    onBeforeMount(async()=>{
        albums.value=await store.getAlbums()
    })
  ```
- Subscribe to store actions : Pinia offers "store.$onAction()" to watch store actions.We can access action name,store name,if an error throw which function must etc.
- as default actions when component unmounted "store.$onaction()" method also unmounted.If we want to keep alive use `store.$onAction(callback,true)` 
```typescript
// index.ts
actions:{
    async getAlbums(){
            const res=await axios("https://jsonplaceholder.typicode.com/albums")
            if(res.status!==200) throw new Error("something went wrong")
            return res.data
    }
}
```
```typescript
//index.vue
<script setup lang="ts">
import { ref } from "@vue/reactivity";
import { useStore } from "../store";

const store=useStore()
const users=ref("")
const unsubscribe =store.$onAction(
  ({
    name, // name of the action
    store, // store instance, same as `someStore`
    args, // array of parameters passed to the action
    after, // hook after the action returns or resolves
    onError, // hook if the action throws or rejects
  }) => {
    // a shared variable for this specific action call
    const startTime = Date.now()
    // this will trigger before an action on `store` is executed*************77777777777
    console.log(`Start "${name}" with params [${args.join(', ')}].`)
console.log(store)
    // this will trigger if the action succeeds and after it has fully run.
    // it waits for any returned promised
    after((result) => {
        users.value=result
      console.log(
        `Finished "${name}" after ${
          Date.now() - startTime
        }ms.\nResult: ${result}.`
      )
    })

    // this will trigger if the action throws or returns a promise that rejects
    onError((error) => {
      console.warn(
        `Failed "${name}" after ${Date.now() - startTime}ms.\nError: ${error}.`
      )
    })
  })
  store.getAlbums()
</script>
```
### Importante errorte : logic of pinia is simple.import store to use a store.When we want to use a state,a getter or an action from another store.Import store and use what you want.
```typescript
// auth.ts
import { defineStore } from "pinia";

export const authStore=defineStore("authStore",{
actions:{
        getMe(){
            console.log("you got me")
        }
    }
})
```
```typescript
//index.ts
import { defineStore } from "pinia";
import { authStore } from "./auth";
export const useStore=defineStore("useStore",{
    actions:{
        withGetMe(){
            const auth=authStore()
            auth.getMe()
        }
    }
})
```

## [Plugins](https://pinia.vuejs.org/core-concepts/plugins.html )
- adding properties to all  stores,adding functions,adding extra features etc.
- ```typescript
  //plugins.ts
  import { PiniaPluginContext } from "pinia"

  export const appPlugin=(context:PiniaPluginContext)=>{
      console.log(context.app) // represent vue app
      console.log(context.options) // the options object defining the store passed to `defineStore()`
      console.log(context.pinia) // the pinia created with `createPinia()`
      console.log(context.store) // we can add extra features to store.state,action or getters
      return {appName:"ilex"}
  }
  ```    
- ```typescript
    import { createPinia } from 'pinia'
  import { createApp } from 'vue'
  import App from './App.vue'
  import router from './router'
  import { appPlugin } from './store/plugins'

  const myApp=createApp(App)
  const pinia=createPinia()
  pinia.use(appPlugin)
  myApp
  .use(router)
  .use(pinia)
  .mount('#app')
    ```
- ```typescript
  context.store["username"]="app" 
  context.store.$state["username"]="app"
  return {"username":"app"} // these are seem equal but as possible as use return or store.#state.username for devtools tracking 
  ```
- if you want to debug it in devtools:`store._customProperties.add('propertyName')`
- when we add a feature through plugin we must declare its type for typescript typing.
 - ```typescript
    import { PiniaPluginContext } from "pinia"
    import "pinia"
    import { Ref } from "vue"
    declare module 'pinia' {
    export interface PiniaCustomProperties {
    // by using a setter we can allow both strings and refs
    set appName(value: string | Ref<string>)
    get appName(): string
    // or
    appName:string
    }
    }
    export const appPlugin=(context:PiniaPluginContext)=>{
    return {appName:"app"}
    }
   ```
- Typing new state properties:
```typescript
declare module 'pinia' {
  export interface PiniaCustomStateProperties<S> {
    appName:string
  }
}
export const appPlugin=(context:PiniaPluginContext)=>{
    context.store.$state.appName="hi bro"
}
```

## [Using pinia outside of components](https://pinia.vuejs.org/core-concepts/outside-component-usage.html)
- to use pinia  It firstly must added to vue.therefore we can use pinia in vue dependencies like navigation guard.(not in vanilla js)
- ```typescript
  import { useStore } from "../store";
    const store=useStore() // err
  router.beforeEach((to,from,next)=>{
    const store=useStore()  // ok
    console.log(store.userExist)  
  })
  ```

## [Server Side Rendering (SSR)](https://pinia.vuejs.org/ssr/)
