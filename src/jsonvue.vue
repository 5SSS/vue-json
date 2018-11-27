<template>
  <div class="alpaca-json">
    <slot v-if="isObject">
      <object-tree :data="data"></object-tree>
    </slot>
    <slot v-else-if="isArray">
      <array-tree :data="data"></array-tree>
    </slot>
    <slot v-else>{{ data || 'null' }}</slot>
  </div>
</template>

<script>
import * as type from './typeof.js'
import objectTree from './objectTree.vue'
import arrayTree from './arrayTree.vue'
export default {
  components: {
    objectTree,
    arrayTree
  },
  props: ['data'],
  computed: {
    isArray () {
      return type.isArray(this.data)
    },
    isObject () {
      return type.isObject(this.data)
    }
  }
}
</script>

<style>
  .alpaca-json {
    width: 100%;
    padding: 10px;
    color: #7c7d7d;
    text-align: left;
    font-size: 14px;
    background-color: #f8f8f8;
    border-radius: 5px;
  }
  .alpaca-p {
    padding-left: 20px;
  }
  .alpaca-finger:hover {
    font-weight: bold;
    cursor: pointer;
    color: #2d8cf0;
  }
  .alpaca-number {
    color: #ae81ff;
  }
  .alpaca-string {
    color: #a6e22e;
  }
  .alpaca-boolean {
    color: #6F73FF;
  }
  .alpaca-null {
    color: #66d9ef;
  }
  .alpaca-undefined {
    color: #f92672;
  }
</style>
