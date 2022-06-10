**Note** we must install sass to using. `npm install -D sass`

## <b id="file-system-routing">[File ststem routing](https://github.com/hannoeru/vite-plugin-pages) </b>: 
- vite-plugin-pages is used for file system routing.
- https://github.com/hannoeru/vite-plugin-pages
- ```typescript
    // vite-env.d.ts 
    /// <reference types="vite-plugin-pages/client" /> // for typing.
  ```
- as default it is uses "src/pages" for route address ,use  dirs:"" for changing.
- **Basic Routing** :
  - ```css
       src>pages>hi.vue === /hi
    ```
- **Index Routing** : index.vue gets the folder.
   - ```css
      src>pages>index.vue === /
      src>pages>user>index.vue === /user
      ```
- **Dynamic Routing** :it gives parameters as prop
   ```css
   src>pages>[id].vue  === /:id
   src>pages>[id]>hi.vue === /:id/hi
   ```
- **catching all unused routes** : 
   ```css
   src>pages>[...all].vue  === /*
   ``` 
- **Nested routes** : 
```
  src/pages/
            hi.vue
            user/
                index.vue
                [id].vue
 ```

```javascript
    [
      {
        path:"/hi",
        component:"/src/pages/users/hi.vue"
      },
    {
      path: '/user',
      children: [
        {
          path: '',
          component: '/src/pages/users/index.vue',
          name: 'users'
        },
        {
          path: ':id',
          component: '/src/pages/users/[id].vue',
          name: 'users-id'
        }
      ]
    }
  ]
  ```
- for using meta tags : 
   - ```javascript
     <route>
      {
        meta: {
          requiresAuth:"aklsdm"
        }
      }
      </route>
     ```

## <b id="vite-plugin-vue-layouts">[vite-plugin-vue-layouts](https://github.com/JohnCampionJr/vite-plugin-vue-layouts) </b> :    
- Provides layout system for vue.Use with [file-system-routing](#file-system-routing)
- **layouth path** : layoutsDirs
     -  default: src/layouts
     -  changing : layoutsDirs : string | string[]
- **default layouth** : defaultLayout
     - default : default
     - changing: defaultLayout: string
- **transition pattern** : 
    ```html
      <template>
        <router-view v-slot="{ Component, route }">
          <transition name="slide">
            <component :is="Component" :key="route" />
          </transition>
        </router-view>
      </template>
    ```
- **client-types** : "types": ["vite-plugin-vue-layouts/client"]
- layout matching: 
    ```javascript
        <route lang="yaml">
        meta:
          layout: default // write layout name in component.As default it  inject default layout.
        </route>
    ```

## **<b id="unplugin-vue-components">[unplugin-vue-components](https://github.com/antfu/unplugin-vue-components#migrate-from-vite-plugin-components)</b>**     
- automatically imports components. so we don't need to type `import Component' from "../src/components/Component.vue".
- **configuration** : 
  - ```javascript
    import Components from 'unplugin-vue-components/vite'

    export default defineConfig({
      plugins: [
        Components({ /* options */ }),
      ],
    })
    ```
- we can use it with UI kits like quasar,vuetify...
- ```html 
  // src/pages/index.vue
    <template>
    <Username></Username>
    </template>
  ```
  ```html
    // it will compile this.
    <template>
    <Username></Username>
    </template>
    <script>
    import Username from './src/components/Username.vue'

    export default {
    components: {
    Username
    }
    }
  </script>
   ```             
- as default it works with  src/components to auto importing. to generate custom paths use `dirs: ['src/components']`

## **[unplugin-icons](https://github.com/antfu/unplugin-icons)**   

- Use over 100,000 icons as component and compile only the icons you use when compiling the project. Icons powered by [Iconify](https://iconify.design/)
- `npm i -D unplugin-icons` 
-  after now on install what you want to use icon set. example : `npm i -D @iconify-json/carbon`
- Use with [unplugin-vue-components](#unplugin-vue-components) to auto import icons.
- We can use custom icons.
- to set properties to all icons by default : 
  - ```javascript
    Icons({
      scale: 1.2, // Scale of icons against 1em
      defaultStyle: '', // Style apply to icons
      defaultClass: '', // Class names apply to icons
      compiler: null, // 'vue2', 'vue3', 'jsx'
      jsx: 'react' // 'react' or 'preact'
    })
    ```
- ```typescript
  // vite.config.ts
  import { defineConfig } from 'vite'
  import Components from 'unplugin-vue-components/vite'
  import Icons from 'unplugin-icons/vite'
  import IconsResolver from 'unplugin-icons/resolver'

  // https://vitejs.dev/config/
  export default defineConfig({
  plugins: [
    Components({ 
      resolvers: [
        IconsResolver({
          prefix:"myIcons"
        }),
      ]
    }),
    Icons()
  ],
    
  })
  ```
- ```html
  <!-- in a page -->
    <template>
    <myIcons-carbon-accessibility/>
    </template>
  ```
- **icon declaration structure** : prefix-collection-iconName
  - prefix : as default its value is "i" . to change it : 
     - ```typescript
        Components({ 
        resolvers: [
        IconsResolver({
        prefix:"myIcons" //here
        }),
        ]
        })
       ```
     - for non-prefix mode write   `prefix: false`

   - collection : Icon set what you used . [Icon set Ids](https://icon-sets.iconify.design/)

## [unplugin-auto-import](https://github.com/antfu/unplugin-auto-import)
- auto importing for provides modules.
- Installing : 
  - `npm i -D unplugin-auto-import`
  - ```typescript
      // vite.config.ts
      import AutoImport from 'unplugin-auto-import/vite'

      export default defineConfig({
        plugins: [
          AutoImport({ /* options */ }),
        ],
      })
  ```
- `imports:string[]` : which modules will we use:  react,vue,vue-router,pinia,quasar etc.
- ```typescript
  // with unplugin-auto-import
  <script setup lang="ts">
  onBeforeMount(()=>console.log("aasd"))
  const user=computed(()=>2)
  const router=useRouter()

  </script>
  ```
- ```typescript
  // without unplugin-auto-import
  <script setup lang="ts">
  import { useRouter } from "vue-router";
  import { onBeforeMount,computed } from "vue";
  onBeforeMount(()=>console.log("aasd"))
  const user=computed(()=>2)
  const router=useRouter()
  </script>
  ```
- as default It write a file on main structure but must be included src directory for typescript typing. `dts:"./src/auto-imports.d.ts",`