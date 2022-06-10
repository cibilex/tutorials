# Grid system
## Parent Properties
- **display** : grid or inline-grid
- **grid-template-columns** :  auto - 20px -2rem - 200px
    - `auto 20px auto`
    - `2fr 20px 1fr`
    - `20px 500px 100px`
- **grid-template-rows** :auto - 20px -2rem - 200px
    - `auto 20px auto`
    - `2fr 20px 1fr`
    - `20px 500px 100px`
- **grid-template** : grid-template-rows / grid-template-columns
- **column-gap** : 20px 1rem 
- **row-gap** : 20px 1rem
- **grid-gap** :  row-gap / column-gap
- **grid-template-areas** : grid map.
    - `grid-template-areas: "A B C" "D E F"`
    - `grid-template-areas: "header header header" "Driwer Content Content" "Driwer Content Content"`
    - for use them :`grid-area : header`
- **justify-content** : `space-around` or start,end,space-around,space-evenly,space-between
- **align-content** : `space-around` or start,end,space-around,space-evenly,space-between
- for using both of them **place-items** : `space-around` or start,end,space-around,space-evenly,space-between . for using both of them




## Child Properties
- **order** : 1 10 200 -1 -5
- **grid-column-start** : a number between the number of grid column corners. 1 2 3 4 5
- **grid-column-end** : a number between the number of grid column corners  . 1 2 4 5 10
- **grid-column** : `grid-columnd-start / grid-column-end`
- **grid-row-start** : a number between the number of grid row corners . 1 2 4 5 6 10
- **grid-row-end** : a number between the number of grid row corners  . 1 2 4 5 6 7
- **grid-row** : `grid-row-start / grid-row-end`
- **grid-area** : grid-row-start / grid-column-start / grid-row-end / grid-column-end or choose an child from  grid-template-areas
- **Note** : `grid-template-row:1 / 3` === ``grid-template-row: 1 / span 2` .span is equal to collect of two numbers.

**Notes**
- repeat(times,value) :
    - `grid-template-columns: repeat(3,auto)` === `grid-template-columns: auto auto auto auto`
- minmax(minValue,maxValue) : value will be between min and max values.