<template>
  <div class="alpaca-json">
    <slot v-if="canParse">
      <p v-if="isObject" @click="parent=!parent" class="alpaca-f">
        { <span v-show="!parent">... }</span>
      </p>
      <p v-else @click="parent=!parent" class="alpaca-f">
        [ <span v-show="!parent">... ]</span>
      </p>
      <tree v-show="parent" :data="parse" :needKey="isObject"></tree>
      <p v-if="isObject" v-show="parent">}</p>
      <p v-else v-show="parent">]</p>
    </slot>
    <slot v-else>{{ data }}</slot>
  </div>
</template>

<script>
import { isObject, isArray } from './typeof.js'
import tree from './tree'
import parseData from './parsing'
export default {
  props: ['data'],
  components: {
    tree
  },
  data () {
    return {
      parse: [],
      isObject: true,
      parent: true
    }
  },
  created () {
    this.parsing()
  },
  watch: {
    data (val) {
      this.parsing()
    }
  },
  computed: {
    canParse () {
      if (isObject(this.data) || isArray(this.data)) {
        return true
      }
      return false
    }
  },
  methods: {
    parsing () {
      if (isObject(this.data)) {
        this.isObject = true
      } else if (isArray(this.data)) {
        this.isObject = false
      } else {
        this.isObject = false
      }
      this.parse = parseData(this.data)
    }
  }
}
</script>

<style>
  .alpaca-json {
    padding: 10px 20px;
    border-radius: 5px;
    color: #032f62;
    background-color: #fafbfc;
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    font-size: 14px;
  }
  .alpaca-p {
    position: relative;
  }
  .alpaca-line {
    position: absolute;
    text-indent: 0;
    left: 5px;
    top: 2px;
    font-size: 12px;
    color: #808695;
    z-index: 5;
    user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    -webkit-user-select: none;
  }
  .alpaca-k {
    color: #5cadff;
  }
  .alpaca-f {
    cursor: pointer;
  }
  .alpaca-f:hover .alpaca-k {
    color: #19be6b;
    font-weight: bold;
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
