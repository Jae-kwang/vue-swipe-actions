# vue-swipe-actions

**iOS style swipe actions for Vue.js, [Live Demo](https://ecollect.github.io/vue-swipe-actions/)** ([Source](https://github.com/eCollect/vue-swipe-actions/blob/master/src/App.vue))

## Installation

```
npm install --save vue-swipe-actions
```

```js
import { SwipeList, SwipeOut } from 'vue-swipe-actions';

export default {
  components: {
    SwipeOut,
    SwipeList
  }
};
```

## Basic Usage

### Import Styles

```javascript
import 'vue-swipe-actions/dist/vue-swipe-actions.css';
```

### SwipeList

SwipeList component is just a helper for listing multiple SwipeOuts.

#### Props

| Prop             | Data Type | Required|Default| Description        |
| ---------------- | --------- |-------- |-------|------------------ |
| `items`          | Array     | *       |       | An array with your data |
| `item-key` | String    |         |id     | Your key for :key when list is v-for-ed, if not found array index will used|
| `disabled`       | Boolean   |         |false  | if true items will be disabled, and text selection will be possible (on desktop). adds class ``swipeout--disabled``  |
| `threshold`      | Number    |         |45     | With that property you can fine tune when actions are considered open |

#### Events

| Event                    | Payload        | Description        |
| ----------------------- | --------------- | -|
| `swipeout:click`        | item 			| Emitted on single click/tap on the item |
| `active`                | Boolean         | Emitted when the user is opening/closing the any of the actions |

#### Methods

| Method                  | Params          | Description        |
| ----------------------- | --------------- | -|
| `revealRight`           | index (number)  | Reveals right actions on given index |
| `revealLeft`            | index (number)  | Reveals left actions on given index |
| `closeActions`          | index (number)? | Closes actions on given index, or all if no index given |


### SwipeOut

SwipeOut is the main component, representing a single item with it's actions.

#### Props

| Prop             | Data Type | Required|Default| Description        |
| ---------------- | --------- |-------- |-------|------------------ |
| `disabled`       | Boolean   |         |false  | if true items will be disabled, and text selection will be possible (on desktop). adds class ``swipeout--disabled``  |
| `threshold`      | Number    |         |45     | With that property you can fine tune when actions are considered open |

#### Events

| Event                    | Payload         | Description        |
| ----------------------- | --------------- | -|
| `active`                | Boolean         | Emitted when the user is opening/closing the any of the actions |

```html
<swipe-list
	ref="list"
	class="card"
	:disabled="!enabled"
	:items="mockSwipeList"
	item-key="id"
	@swipeout:click="itemClick"
>
	<template v-slot="{ item, index, revealLeft, revealRight, close }">
		<!-- item is the corresponding object from the array -->
		<!-- index is clearly the index -->
		<!-- revealLeft is method which toggles the left side -->
		<!-- revealRight is method which toggles the right side -->
		<!-- close is method which closes an opened side -->
		<div class="card-content">
			<!-- style content how ever you like -->
			<h2>{{ item.title }}</h2>
			<p>{{ item.description }}</p>
			<span>{{ index }}</span>
		</div>
	</template>
	<!-- left swipe side template and v-slot:left="{ item }" is the item clearly -->
	<!-- remove if you dont wanna have left swipe side  -->
	<template v-slot:left="{ item, close }">
		<div class="swipeout-action red" title="remove" @click="remove(item)">
			<!-- place icon here or what ever you want -->
			<i class="fa fa-trash"></i>
		</div>
		<div class="swipeout-action purple" @click="close">
			<!-- place icon here or what ever you want -->
			<i class="fa fa-close"></i>
		</div>
	</template>
	<!-- right swipe side template and v-slot:right"{ item }" is the item clearly -->
	<!-- remove if you dont wanna have right swipe side  -->
	<template v-slot:right="{ item }">
		<div class="swipeout-action blue">
			<!-- place icon here or what ever you want -->
			<i class="fa fa-heart"></i>
		</div>
		<div class="swipeout-action green">
			<!-- place icon here or what ever you want -->
			<i class="fa fa-heart"></i>
		</div>
	</template>
	<template v-slot:empty>
		<div>
			<!-- change mockSwipeList to an empty array to see this slot in action  -->
			list is empty ( filtered or just empty )
		</div>
	</template>
</swipe-list>
```

```js
export default {
  components: {
    SwipeOut,
    SwipeList
  },
  data() {
    return {
      enabled: true,
      mockSwipeList: [
        {
          id: 0,
          title: "Some title",
          description: "some description"
        },
        {
          id: 1,
          title: "Some title",
          description: "some description"
        },
        {
          id: 2,
          title: "Some title",
          description: "some description"
        }
      ]
    };
  },
  methods: {
    revealFirstRight() {
      this.$refs.list.revealRight(0);
    },
    revealFirstLeft() {
      this.$refs.list.revealLeft(0);
    },
    closeFirst() {
      this.$refs.list.closeActions(0);
    },
    closeAll() {
      this.$refs.list.closeActions();
    },
    remove(item) {
      this.mockSwipeList = this.mockSwipeList.filter(i => i !== item);
      // console.log(e, 'remove');
    },
    itemClick(e) {
      console.log(e, "item click");
	},
    fbClick(e) {
      console.log(e, "First Button Click");
    },
    sbClick(e) {
      console.log(e, "Second Button Click");
	},
  },
}
```

### Styling

The default styling is as minimal as possible, defining no visual styles but only functional ones.
You can overwrite any of the styles if needed.

```css
.swipeout {
  position: relative;
  overflow: hidden;
  user-select: none;
  display: flex;
}
.swipeout.swipeout--disabled {
  user-select: auto;
}
.swipeout .swipeout-left,
.swipeout .swipeout-right {
  position: absolute;
  height: 100%;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  z-index: 1;
}
.swipeout.swipeout--transitioning .swipeout-action,
.swipeout.swipeout--transitioning .swipeout-content {
  transition: transform 0.3s;
}
.swipeout .swipeout-content {
  width: 100%;
}
.swipeout .swipeout-action,
.swipeout .swipeout-content {
  will-change: transform;
}
.swipeout .swipeout-left {
  left: 0;
  transform: translateX(-100%);
}
.swipeout .swipeout-right {
  right: 0;
  transform: translateX(100%);
}
.swipeout-list-item {
  outline: none;
}
```

## Author

&#169; 2018 eCollect AG.
