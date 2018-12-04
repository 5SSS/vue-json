<template>
  <div class="object-tree">
    <p v-if="showLeaf" @click="parent=!parent" class="alpaca-finger">
      [ <span v-show="!parent">... ]</span>
    </p>
    <p class="alpaca-p" v-show="parent" v-for="(item, index) in data" :key="index">
      <slot v-if="isArray(item)">
        <p @click="toggle(index)" class="alpaca-finger">
          [
          <span v-show="!child[index]" v-if="isLast(index)">... ]</span>
          <span v-show="!child[index]" v-else>... ],</span>
        </p>
        <arrayself v-show="child[index]" :data="item" :showLeaf="false"></arrayself>
        <span v-show="child[index]" v-if="isLast(index)">]</span>
        <span v-show="child[index]" v-else>],</span>
      </slot>
      <slot v-else-if="isObject(item)">
        <p @click="toggle(index)" class="alpaca-finger">
          {
          <span v-show="!child[index]" v-if="isLast(index)">... }</span>
          <span v-show="!child[index]" v-else>... },</span>
        </p>
        <object-tree v-show="child[index]" :data="item" :showLeaf="false"></object-tree>
        <span v-show="child[index]" v-if="isLast(index)">}</span>
        <span v-show="child[index]" v-else>},</span>
      </slot>
      <slot v-else>
        <!-- 最后一项不需要逗号分隔 -->
        <span :class="getClass(item)" v-if="isLast(index)">{{ isNullOrUndefined(item) }}</span>
        <span :class="getClass(item)" v-else>{{ isNullOrUndefined(item) }},</span>
      </slot>
    </p>
    <p v-if="showLeaf" v-show="parent">]</p>
  </div>
</template>

<script>
import * as type from './typeof.js'
export default {
  name: 'arrayself',
  components: {
    objectTree: () => import('./objectTree.vue')
  },
  props: {
    data: {
      type: Array,
      default: () => []
    },
    showLeaf: {
      type: Boolean,
      default: true
    }
  },
  data () {
    return {
      parent: true,
      child: {}
    }
  },
  watch: {
    data (val) {
      this.child = {}
      this.data.forEach((item, index) => {
        if (type.isObject(item) || type.isArray(item)) {
          this.$set(this.child, index, true)
        }
      })
    }
  },
  created () {
    this.data.forEach((item, index) => {
      if (type.isObject(item) || type.isArray(item)) {
        this.$set(this.child, index, true)
      }
    })
  },
  methods: {
    isArray (item) {
      return type.isArray(item)
    },
    isObject (item) {
      return type.isObject(item)
    },
    isLast (index) {
      return (this.data.length - 1) === index
    },
    isNullOrUndefined (item) {
      if (type.isNull(item)) {
        return 'null'
      }
      if (type.isUndefined(item)) {
        return 'undefined'
      }
      // 其他
      return item
    },
    getClass (item) {
      if (type.isNumber(item)) {
        return 'alpaca-number'
      }
      if (type.isString(item)) {
        return 'alpaca-string'
      }
      if (type.isBoolean(item)) {
        return 'alpaca-boolean'
      }
      if (type.isUndefined(item)) {
        return 'alpaca-undefined'
      }
      if (type.isNull(item)) {
        return 'alpaca-null'
      }
      // 其他不改变颜色
      return ''
    },
    toggle (index) {
      this.child[index] = !this.child[index]
    }
  }
}
</script>
