# Vue Js for React Developers

**Table of content**

- [Vue and React are Similar and different](#vue-and-react-are-similar--different)
- [Creating a component](#creating-a-component)
  - [The Structure of a Single-File Component](#the-structure-of-a-single-file-component)
  - [ref: Vue's version of "useState"](#ref-vues-version-of-usestate)
  - [The Double-Curly Braces](#the-double-curly-braces)
  - [Value: Vue’s version of “state” and “setState”](#value-vues-version-of-state-and-setstate)
  - [Reactive](#reactive)
  - [Other Script Syntax](#other-script-syntax)
  - [Custom Hook](#custom-hook)
- [Template Basics](#template-basics)
- [Event Handling](#event-handling)
- [Dynamic & Scoped Style](#dynamic-and-scoped-style)

- [The Fundamental Difference between Vue and React](#the-fundamental-difference-between-vue-and-react)
- [Computed Properties](#computed-properties)
	- [Other functions: watch and watchEffect](#other-functions)
- [Recommended IDE Setup](#recommended-ide-setup)

## Vue and React are similar & different

Vue and React are very similar because they solve the same problem:

- single-page app development
- with stateful components
- powered by VDOM

This two technology are also very different because they solve the problem in different way.

- React: Minimalist library-style approach
- Vue: Full fledged framwork-style approach

**Similarities**

- Component and Reactivity
- Template Basics
- Event Handling
- Dynamic Style

**Differences**

- Computed Properties
- Props, Custom Events, and Slots
- Two-Way Data Binding
- Lifecycle Hooks

<hr>
<br>

## Creating a component

### Some Prep Work

In this lesson, we’re finally going to write some Vue code.

Let’s first remove all the default files in the `src/components` folder.

Also remove all the code inside the `src/App.vue` file. We’re going to create our first Vue.js component from scratch.

### The Structure of a Single-File Component:

Similar to a React component, there are two distinct parts of a Vue component. The script part and the template part. Optionally, there’s a style part for your CSS code, but for now let’s just focus on the script and the template.

**src/App.vue**

```html
<script>
</script>

<template>
</template>
```

To use the latest syntax in `<script>`, we have to add the `setup` attribute. (I’ll explain the syntactical differences between the new and old later in the lesson)

**src/App.vue**

```html
<script setup>
</script>
```

we're going to put all the components logic in the `<script>` section and put the template code in the `<template>` section.

### ref: Vue's version of "useState"

```javascript
<script setup>
import {ref} from "vue";
const product = ref("Socks");
</script>
```

`ref` is the Vue.js equivalent of `useState`.

"ref" stands for reference. The idea is that a value such as a string or a number is not a reference. but we need a reference object to facilitate the reactivity in a Vue component.

For example, a variable with a string value:

```javascript
let product = "Socks";
```

If we are using the variable as a state, changing it means reassigning it:

```javascript

product = “New Socks”

```

The problem is that the original value shocks would be lost after the reassignment if the original value is lost reactivity is lost as well.

views way of getting around this problem is to wrap the value in an object with `ref`

```JavaScript

const product = ref(“Socks”);

```

So our state is technically the `ref` object. not the string value it contains.

Now to render this `ref` state, We will use the double curly braces in the templates to create a binding

**src/App.vue**

```javascript
<template>
  <h1>{{ product }}</h1>
</template>
```

So whenever `product` is changed the `h1` will be updated with its new value if you check the browser you should see `Socks`

### The Double-Curly Braces

The double curly braces are similar to react single curly braces with JSX. Technically they put any JavaScript expression inside them. it doesn't have to be variable.

### Value: Vue’s version of “state” and “setState”

If we want to access or change the value of the ref in `<script>` we have to use the value property on the ref:

**src/App.vue**

```javascript
setTimeout(() => {
	product.value = “New Socks”
}, 1000)
```

The `value` Property is used for both read and write. So it's the _Vue.js equivalent of state and setState_.

In the example here, we are assigning `value` to a new string to change the state. In the browser, you should see the value changing after the one-second time out. But in the `<template>` you don't have to use the `value` property because it will be added for you automatically.

### Reactive

Another way to create a state in Vue is to use `reactive()`. but this is intended for object-type data.

**src/App.vue**

```javascript

import { reactive } from 'vue'

// pass an object to reactive
const product = reactive({
	name: "Socks"
})

...
<h1>{{product.name}}</h1>

```

The main difference is that you don't have to use the value property to access the change its content in both the `<script>` and the `<template>`.

**src/App.vue**

```javascript
setTimeout(() => {
  product.name = "New Socks";
}, 1000);
```

We use `ref()` or `reactive()`, it all depends on your need. if you just need a state for a simple value, then `ref()` should be fine. if you have a bunch of related that usually go together you might want to put them in the same object using `reactive()`.

### Other script syntax

as I mentioned previously using the `setup` attribute on the `<script>` tag means where using the latest Syntax for the component script.

This is called the **Composition API** with the `script setup` syntax:

```javascript

<script setup>
import { ref } from 'vue'

const product = ref("Socks");

setTimeout(() => {
	product.value = "New Socks"
}, 1000)

</script>

<template>
	<h1>{{ product }}</h1>
</template>

```

The above is basically a simplified Syntax on the Classic composition API syntax**:**

```javascript
<script>
import { ref } from 'vue';

export default {
	setup() {
		const product = ref("Socks");

		setTimeout(() => {
			product.value = "New Socks"
		}, 1000)

		return { product }
	}
}
</script>

<template>
	<h1> {{ product }} </h1>
</template>
```

As you can see this is tedious to read/write. that's why we are using the `script setup` Syntax instead.

So from now on I am going to refer to the script setup Syntax as a composition text

in contrast to the composition API, there is the **Options API**. it's yet another way for constructing the component logic

```javascript
<script>
	export default {
		data() {
			return {
				product: "Socks"
			}
		}
		created() {
			setTimeout(() => {
				this.product = "New Socks"
			}, 1000)
		}
	}
</script>

<template>
	<h1> {{ product }} </h1>
</template>
```

### Custom Hook

You can think of the **Composition API** as the Vue.js equivalent of React Hooks.

You can think of the **Options API** as the wages equivalent of React’s class-based API.

That means using the Composition API, we can create custom hooks. But in the Vue.js worlds, We call them _composables_ it's the same concept if you have created custom hooks in React, This should feel right at home

**src/App.vue**

```javascript
// a composable that changes the value after a specified delay
const useChangeWithDelay = function (state, newVal, delay) {
  setTimeout(() => {
    state.value = newVal;
  }, delay);
};

const product = "Socks";

useChangeWithDelay(product, "New Socks", 1000);
```

We're using the "use" prefix to name the composable just like the naming convention of a React hook.

<hr>
<br>

## Template Basics

The most common operations which are used in the `<template>`

- Attribute Binding
- Conditional Rendering
- List Rendering
- List Rendering with Conditional

<hr>
<br>

## Event Handling

event handling using `v-on` or `@` directive.

Here some event modifiers:

| Event Modifiers | Description                                                             |
| :-------------- | ----------------------------------------------------------------------- |
| @click.once     | Make sure the event can be triggered **only once** on the same element. |
| @click.prevent  | Prevents default native, like **event.preventDefault()** in React.      |
| @click.stop     | Stops the event from propagating further up.                            |
| @keydown.enter  | Tergets the 'Enter' key's keydown event.                                |

<hr>
<br>

## Dynamic and Scoped Style

```javascript

// class binding
<div :class="{activeClass : isActive}"></div>

// class binding: multiple classes
<div class="circle-color" :class="{className : isActive}"></div>
<div class="[color bgColor borderStyle]"></div>

// class binding: ternary operators
<div :class="[isActive ? 'isActive' : '']"></div>

// class binding
<div :class="!inStock ? 'disabledButton' : ''"></div>

// Camelcased property name in style object
<div :style="{backgroundColor: variant.color}"></div>

// kebab-cased property name in style object
<div :style="{'background-color': variant.color}"></div>

// style binding: object (index.html)
<div :style="styles"></div>
// (main.js)
data(){
  return{
    styles: {
      fontSize: '20px',
      color: 'red',
    }
  }
}


```

The style tag is just simple, you just put css in it and it works. To make it even more useful, you can use the `scoped` attribute:

**src/App.vue**

```javascript
<script>
	import ChildComponent from './components/ChildComponent.vue';
</script>

<template>
	<ChildComponent />
</template>

<style scoped>
	p{
		border: 2px solid red;
	}
</style>
```

**src/components/ChildComponent.vue**

```javascript
<script>

</script>

<template>
	<p>
		<p>Hello</p>
		<p>World</p>
	</p>
</template>
```

Now, the css will be scoped to only the current component and not affect its child components, except the root element of child component. Here, Only the top-most `<p>` element of the child component will be affected by the red border style.

<hr>
<br>

## The Fundamental Difference between Vue and React

Vue is using `ref` while React is using `useState`. These functions are basically the same thing but just with different names and syntax. But in terms of runtime execution, they are very different. 
The code inside a React component will get executed again and again as part of the reactive progress.

```javascript
import { useState } from 'react'

export default function() {
	const [count, setCount] = useState();
	console.log("This will be logged many times.")
	
	return <button onClick={() => setCount(count + 1)}>{count}</button>
}
```
But on the other hand, the Vue.js script block only runs once.

```javascript
<script setup>
import { ref } from 'vue'

const count = ref(0);
console.log("This will be logged only once.")
</script>
<template>
	<button @click="() => count++">{{ count }}</button>
</template>
```
That's because the Vue.js Framework was implemented differently, notably with the observer pattern.

A Vue.js state is basically an object that can be subscribed to. And the subscriber will get notified whenever a state change occurs. For example, The template of the component is an implicit subscriber of the state in the same component. That's why when a state is changed, the template will re-run itself. And that's why the `<script>` doesn't need to be run again and again.

<hr>
<br>


## Computed Properties

Computed property would be an example of explicit subscriber. 

The `computed` function takes a callback as a argument. Inside the function, we're returning a new string based off the values of our `brand` and `product`. This will become the value of the computed property.

And from now on, whenever `brand` or `product` is changed, the value of `title` will get recalculated automatically.

This is different from React's approach, where the entire component function would get executed every time something is changed. In Vue.js, only the callback of the computed property would get re-executed.

The `computed` function is a part of Vue 3 Composition API, so you can also use it inside a composable

### Other functions

The composition API also provides a few other useful functions for *reacting to state changes*, such as `watch` and `watchEffect`.

**watch**
The `watch` function can help you to run a callback whenever a particular state is changed

```javascript
import { ref, watch } from 'vue'

const count = ref(0);

watch(count, () => {
	// do something when count is changed
})
```

There's no need to return anything from this callback function.

**watchEffect**

`watchEffect` is similar to `watch`, but you don't have to specify what to watch. Any *subcribable* object that appears inside the callback will be subscribed to automatically. This is useful for subscribing to multiple states, and you don't want to manually list out all of them.

```javascript
import { ref, watchEffect } from 'vue'

const a = ref(0);
const b = ref(0);

watchEffect(() => {
	console.log(a.value, b.value)
})
```

But different from `watch`. `watchEffect` will run the callback once from start before any state change, `watch` will only run the callback when the source is changed.

Like `computed`, `watch` and `watchEffect` can be used in your own custom composables.

<hr>
<br>

## Recommended IDE Setup

This template should help get you started developing with Vue 3 in Vite.

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur).

### Customize configuration

See [Vite Configuration Reference](https://vitejs.dev/config/).

### Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```
