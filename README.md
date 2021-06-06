# ellipsis-textarea
基于textarea实现的文本省略vue组件，支持设置最大显示行数

- ellipsis text in a smart way
- support limiting rows

![image](https://user-images.githubusercontent.com/31173307/120924607-0a832f00-c707-11eb-9f17-2ceb331ba8f8.png)


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
