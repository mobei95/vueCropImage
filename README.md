# vue-crop-image

> 一个基于VUE框架的图片裁剪组件

## 安装
``` bash
npm install --save vue-crop-image
```

## 使用
```bash
<template>
  <vue-crop
    :src="imgUrl"
    :aspectRatio="16/9"
    @copperImg="getImg">
  </vue-crop>
</template>
<script>
import vueCrop from 'vue-crop-image'
export default {
  components: {
    vueCrop
  }
}
</script>
```

## Options

| Name           | Type     | Required | default  | Description                     |
| -------------- | -------- | -------- | -------- | ------------------------------  |
| src            | `string` | true     | --       | 需要裁剪的图片地址                |
| aspectRatio    | `number` | --       | 1(1/1)   | 裁剪框的比例,默认为1:1            |

## Event

| Name                 | Description      | Callback Parameter              |
| -------------------- | ---------------  | ------------------------------  |
| copperImg            | 裁剪框发生变化    | 裁剪后的图片(Base 64位)          |


