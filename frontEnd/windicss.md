- **Features**
    - **defineConfig** : to control properties and autocomplete.
        -  ```typescript
            import { defineConfig } from 'windicss/helpers'

            export default defineConfig({
            /* configurations... */
            })
           ```
    - **Shortcuts** : for shortcut classes.
         - ```typescript
            shortcuts:{ // btn and btn-active classes will corresponding these features.
            btn:"text-center  text-light-300 bg-dark-300 py-sm px-xl font-bold font-italic capitalize",
            "btn-active":"text-dark-300 bg-light-300 text-40px"
            } 
           ``` 
    - **Variant-groups** : `hover:(text-green-200 opacity-30 bg-light-300)`
    - **Responsive design** : 
         - sm:if greater than sm
         - <sm:if lesser than sm
         - @sm: if between sm
    - **Dark mode** : `dark:bg-dark-400` `dar:(bg-light-400 text-center grid )` `light:bg-light-200`
       - windi.config.ts > `{darkMode: 'class | media',}`
             - 'media' : darkmode will config by user system preference
             - 'class' : darkmode will config by html class.
         - changing dark mode : 
            - add: `document.documentElement.classList.add('dark')` 
            - remove: `document.documentElement.classList.add('light')`
        - ```typescript
            if (window.matchMedia('(prefers-color-scheme: dark)').matches)
            document.documentElement.classList.add('dark')
            else
            document.documentElement.classList.add('light')
          ```
    - **Important prefix** : `!text-center !absolute`
  











## Grid system
- **grid**
- **inline-grid**
- **grid-cols-{sizes}** : `grid-cols-4` -  `grid-cols-[1fr,2fr]`
- **grid-rows-{sizes}** : `grid-rows-4`  - `grid-rows-[1fr,2fr]`
- **col-span-{number}** : `col-span-full` -  `col-span-2`- `col-span-6`
- **row-span-{number}**  : `row-span-full` - `row-span-2` - `row-span-6`
- **col-start-{number}** : `col-start-1` - `col-start-3`
- **col-end-{number}** : `col-end-2` - `col-end-3`
- **row-start-{number}** : `row-start-2` - `row-start-5`
- **row-end-{number}** : `row-start-4` - `row-start-2`
- **gap-{size or number}** : `gap-20` - `gap-1rem`
- **gap-x-{size or number}** : `gap-x-20` - `gap-x-1rem`
- **gap-y-{size or number}** : `gap-y-20` - `gap-y-1rem`


## Grid Position
- **order-{number}** : `order-1` - `order-21`
- **justify-{style}** : `justify-evenly` or between ,around,center,start,end
- **content-{style}** : `content-evenly`or between ,around,center,start,end
- for using both of them : **place-content-{style}** : `place-content-evenly` or between ,around,center,start,end
- **Note**                               
    -  content : to position the cells by column or row.
    -  items : to position the cells by own position.
- **justify-items-{style}** : `justify-items-start` or end,center,stretch
- **align-items-{style}** : `align-items-start` or end,center,stretch
- for using both of them : **place-items-{style}** : `place-items-stretch` or start,end,center
- **justify-self-{style}**   : `justify-self-start` or end ,center, stretch
- **align-self-{style}**  : `align-selft-start` or end ,center, stretch.
- for using both of them : **place-self-{style}** : `place-self-start` or end ,center, stretch.
- **col-span-{number}** : `col-span-2`
- **row-span-{number}** : `row-span-3`
 
