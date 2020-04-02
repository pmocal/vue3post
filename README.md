# Vue 3.0

Vue 3.0 is coming out soon. Expect a faster, smaller, more maintainable, and easier to use version of the Vue you know and love. You can still use Vue via a script tag and your Vue 2.x code will continue to work. But you can start playing with the alpha version of Vue 3.0 [here](https://github.com/vuejs/vue-next) and we're going to get into some of what 3.0 is offering. Among other things, there is a new API for creating components. It doesn't introduce new concepts to Vue, but rather exposes Vue's core capabilities like creating and observing reactive state as standalone functions. This is ultimately useful to Vue developers at all levels.

## Options API and Composition API

In Vue 2, components are created with the object-based Options API. Vue 3 adds a set of APIs, referred to as the [Composition API](https://vue-composition-api-rfc.netlify.com/), which is function-based. This is primarily to address two issues that Vue 2 ran into for very large projects.

In large components that encapsulate multiple logical tasks, you want to group code by feature, but the nature of the Options API is that such code gets split up (among lifecycle hooks and so on), negatively affecting readability. Secondly, you want to be able to reuse logic in large scale projects, and in Vue 2, solutions like mixins don't address either issue very well.

Vue 3 seeks to kill both birds with one stone by exposing a new API. This API will live alongside the Options API, not replace it. This means that you can go on building components in the way that you're used to without encountering any problems. But, you can also start building with the Composition API which provides more flexible code organization and logic reuse capabilities as well as other improvements. Even if the problems it specifically addresses are not pertinent to you, the new API has clearly had a lot of thought go into it to push Vue forward as a framework period (for instance, by reducing the extent to which Vue operates "magically" behind the scenes).

## Composition API 

The Composition API is available now as a [plugin](https://github.com/vuejs/composition-api) for Vue 2.0 so you can try it out. It will be shipped built in to Vue 3.0 of course.

In Vue 2 reactivity was achieved through the getters and setters of `Object.defineProperty`. This caused some limitations which you have all probably experienced (such as updating an Array by index). In Vue 3, reactivity is accomplished through [proxies](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy), a feature that was introduced in JavaScript ES6.

You need not have a Vue instance to use the new reactivity API. It offers standalone APIs which allow you to create, observe, and react to state changes.

You would first `import { reactive } from 'vue'`. Then, you could create an object in the following way:

`const state = reactive({ count: 0 })`

You will have access to APIs that will allow you to dynamically inject component lifecycle hooks into a Vue instance.

The lifecycle registration methods can only be used in the `setup()` method which is the entry point where all the composition functions are called. For instance:

```vuejs
import { onMounted } from 'vue'
export default {
	setup() {
		onMounted(() => {
			console.log('component is mounted.')
		})
	}
}
```

Functions that use these APIs can be imported into a component, allowing it to do multiple logical tasks with reusable and readable code.

## Typescript

The composition API also offers better TypeScript support. It is supposed to result in better type inferences with bindings returned from `setup()` and `props` declarations used to infer types.

Component code using TypeScript and JavaScript will look largely identical and TypeScript definitions benefit Javascript users as well, say, if they use an IDE.

## View declaration

Vue 2 supports templates as well as render functions. You don't need to know an awful lot here except that Vue 3 continues to support both while optimizing rendering speed (such as by speeding up `diff` algorithms that operate under the hood so that Vue knows what needs to be rerendered).

## Faster

Virtual DOM has been rewritten from the ground up to make for faster mounting and patching.

Compile-time hints have been added to reduce runtime overhead. This means skipping unnecessary condition branches and avoiding re-renders. Static tree and static prop hoisting means entire trees and nodes can skip being patched. Inline functions (like in a handler for a component in a template) won't cause unnecessary re-renders.

You're going to get a proxy-based observation mechanism with full language coverage and better performance. Instance properties will be proxied faster using native Proxy instead of `Object.defineProperty` like old.

You can expect up to 100% faster component instance initialization with double the speed and half the memory usage.

## Smaller

Vue 3.0 is also **smaller**.

It is tree-shaking friendly--tree shaking refers to shaking off unused code. The core runtime has gone from ~20kb in size gzipped to ~10kb gzipped.

The size of the Vue bundle increases with each new feature but, by providing most global APIs and Vue helpers as ES module exports, Vue 3 makes more code tree-shakeable, even template code.

## Coherence

Libraries like Vue Router and test-utils will be updated to line up with the new Vue. Vue now has a custom renderer API (similar to [React Native](https://reactnative.dev/) for those who want to use it to create renderers for mobile or other host environments.

## Conclusion

There is a ton to look forward in Vue 3.0 with [more](https://github.com/vuejs/rfcs/pulls?q=is%3Apr+is%3Amerged+label%3A3.x) like [Portals](https://github.com/LinusBorg/portal-vue) that couldn't fit in this post. The new Composition API moves us towards an all around better Vue. An exact release date is not set but it is coming soon. Get a head start now!
