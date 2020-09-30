# CoreUI Minimalist Guide
This guide made to explain how to use CoreUI on Vue Environment as simple as possible, note we try to make it simple, but dont expect it will be short.
## Table
>We will use table.vue and tables.vue that can be found in **src/views/base** as example

### Datatable
while there are a lot element in table template, we will talk about how to display data using datatable instead
* You dont really need to change anything in template beside datatable, except you want to make some changes on it like adding new element. in that case, it need deeper explanation so it will not discussed here.

Datatable can be used by declared `<CDataTable>` and needed to provide some attribute(props), the most important props is 'fields' and 'item'

Field define a table row that will be loaded in datatable.

while, item is the data

you can define datatable props in script
```
<script>
export default {
  name: 'Table',
  props: {
    items: Array,
    fields: {
      type: Array,
      default () {
        return ['username', 'registered', 'role', 'Status']
      }
    }
}
</script>
```
Above is example of datatable script, it have a item with array data type but without provide any data(that mean component/view that use it a.k.a *tables.vue* need to provide the data), 
and a field that have a default value as described (there no need to provide it at *tables.vue* except you want to load different field value)

in template. don't forget to add it in datatable so it will looks like this:

```
<CDataTable
        :items="items"
        :fields="fields"/>
```
* quick tip, use (self closing element)

`<CDataTable/> `

if u didn't plan to add any element inside it, otherwise use 
```
<CDataTable>
	....
</CDataTable>
```

### Call Table template

We already talk about datatable in table template, now we will talk how we call *table.vue* on *tables.vue*.
First, we need to import *table.vue*

in script

```
import CTableWrapper from './Table.vue'
```

now we will register CTableWrapper as component

```
export default {
  name: 'Tables',
  components: { CTableWrapper }
}
```

with this, now we can use CTableWrapper in template, by declaring CTableWrapper element

```
<template>
<CTableWrapper
          :items=""
        />
</template>
```

before continue, lets review about datatable props in *table.vue*. 

we already talk that there is two crucial props for datatable, field and item.

in *table.vue*, we already state default value for field so we didn't need to specify the field value here. 
but we still need to give data for datatable to load a.k.a item.

here, we will use usersData located in **src/views/users**.

in script:

```
import usersData from '../users/UsersData'
```

now we already import data from userData, but we still need declare it so template will know about it.

```
export default {
  name: 'Tables',
  components: { CTableWrapper },
  data () {
    return {
      usersData
    }
  }
}
```

great, now lets update CTableWrapper in template.

```
<CTableWrapper
          :items="usersData"
        />
```

now table will display data we provided from usersData.

lastly, what if you want to process data before show it in table?, like adding filtering. it can be achieved by using a function declared in 'methods'

example:

```
export default{
...
methods: {
    shuffleArray (array) {
		//switching data place in array to make it in random order
      for (let i = array.length - 1; i > 0; i--) {
        let j = Math.floor(Math.random() * (i + 1))
        let temp = array[i]
        array[i] = array[j]
        array[j] = temp
      }
      return array
    },

    getShuffledUsersData () {
	//call shuffleArray function that return a randomize of userData
      return this.shuffleArray(usersData.slice(0))
    }
  }
}
```

function above will shuffle data in random, to call the function, its quite simple:

```
<CTableWrapper
          :items="getShuffledUsersData()"
        />
```

now table will show data that already shuffled.

Finally, we done talking about tablle,
for more details:https://coreui.io/vue/docs/components/table.html


# Icon

For this section, we will talk about icon loaded on browser tab that we will call **favicon**, and the one used inside website, we will call it **logo**.

## Favicon

it easy to do this one, look at public folder, you will find a lot of file there. that the icon. to change that, just replace it with a your icon file, you can try to check this site to generate icon file:
https://realfavicongenerator.net/

## Logo

Look at sidebar or header, you will find CoreUI Logo there, we will use `<CIcon>` to render icon.

first, lets talk about CIcon, there are 3 ways to set a source but we will talk 2 of them.

### Using name

```
<CIcon
        name="logo"
      />
```

above will load a icon with name 'logo'.

to make it works we need to register logo icon in global variable. 

to do this open icons.js located in **src/assets/icons**, 

this is where all icon will be registered, you will notice a lot icon already imported and registered. 

to register an icon, check code:

```
export const iconsSet= Object.assign(
....
)
```

this is where we will register icon, take a look code above it, you will find:

`import {logo} from './logo'`

this code mean we will import a logo from logo.js in same directory,
after that inside export, there will be

```
export const iconsSet= Object.assign(
	{logo},
	....
)
```

this code line will make logo registered in global scope, so CIcon will find logo with just assign logo to 'name' attribute.

* note: try to open logo.js to learn more.

* in short, it will tell yout that CIcon need a js array to able to load a icon, given you are using a 'name' attribute to load it.
if you didn't want or didn't know how to convert your icon into js array, you might consider next option.

### Using src

`<CIcon src="/logo.png" height="48" alt="Logo"/>`

this one is more simple, 

it only needed to place your icon in public directory, 

and give the path to src attribute, 

CIcon then will render icon as img,

example above will tell CIcon to load logo.png located in public directory with heigh 48px
