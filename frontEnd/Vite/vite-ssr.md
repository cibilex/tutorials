```typescript
    import { onServerPrefetch } from '@vue/runtime-core'
    onServerPrefetch( ()=>{
      console.log("hi from server")
    })
```

## Pinia
- Pinia SSR kullandığımızda otomatik algılar ve yine storu kullanabiliriz ancak server tarafında store üzerinde değişiklik yapılırsa bu değişiklik cliente aktarılamaz.Bu nedenle initialState ile piniayı serverden alırız.
- güvenlik açısından nuxt/devalue kullan.
```typescript
import devalue from '@nuxt/devalue'
export default viteSSR(
  App,
  {
    routes,
    transformState(state) {
      return import.meta.env.SSR ? devalue(state) : state
    },
  },  
  ({ initialState }) => {
    // ...
    if (import.meta.env.SSR) {  // ssr kısmında pinia initialState atanır.Initial state backendtten frontende veri aktarımını sağlar.
      initialState.pinia = pinia.state.value
    } else { // client sidex
      pinia.state.value = JSON.parse(initialState.pinia)
    }
  }
)
```

## node-fetch :
- hem server hemde client kısmında çalışması için kullan.


## Errors
1. ```
    C:\Users\manku\OneDrive\Masaüstü\New folder\my-vue-app\node_modules\vite-plugin-pages\dist\index.js:694
          if (!userOptions.resolver && config.plugins.find((i) => i.name.includes("vite:react")))
    TypeError: Cannot read properties of undefined (reading 'includes')
    ```
    `Pages({resolver:"vue
