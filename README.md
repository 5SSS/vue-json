# vue-json

[![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/leezng/vue-json-pretty/blob/master/LICENSE)

A JSON component or plugin for vue(2.x), support spread and shrinkage, The highlighted rendering JSON data as a tree structure.

## Links

- [Github](https://github.com/5SSS/vue-json)

## Install

```js
npm install --save vue-json-cool
```

## Usage

```html
<template>
  <div>
    <vue-json-cool :data="jsonData"></vue-json-cool>
  </div>
</template>
```

```js
import vueJsonCool from 'vue-json-cool'

export default {
  components: {
    vueJsonCool
  },
  data () {
    return {
      jsonData: {
        number: 1,
        string: 'hello world',
        boolean: true,
        object: {
          key: 1
        },
        array: [1, '2', true, null, undefined],
        null: null,
        undefined: undefined
      }
    }
  }
}
```

## Props

| Attribute | Level | Description | Type | Default |
|-------- |-------- |-------- |-------- | -------- |
| data | basic | json data | JSON object or object Array, {...}, [{...}, {...}] | - |

## Events

not yet...

## PS

如果喜欢请给个星星，谢谢。
If you like, please give me a star, thank you.

如果需要帮助: QQ:1573815240 邮箱: 1573815240@qq.com
if you need help: QQ:1573815240 email: 1573815240@qq.com