# ellipsis-textarea
ellipsis text in a smart way
## install
```
npm install ellipsis-textarea --save
```
## import
```
import "ellipsis-textarea/dist/EllipsisTextarea.css"
import EllipsisTextarea from "ellipsis-textarea"
Vue.use(EllipsisTextarea)
```
## usage
```
<template>
  <div class="page">
    <ellipsis-textarea :text="text" :maxRows="2"></ellipsis-textarea>
  </div>
</template>

<script>
export default {
  data() {
    return {
      text: "一轮巨大的水淋淋的鲜红月亮从村庄东边暮色苍茫的原野上升起来时，村子里弥漫的烟雾愈加厚重，并且似乎都染上了月亮的那种凄艳的红色。",
    };
  },
};
</script>
```
## example
```
see App.vue
```
