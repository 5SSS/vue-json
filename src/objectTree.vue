<template>
  <div class="object-tree">
    <p v-if="showLeaf" @click="parent=!parent" class="alpaca-finger">
      { <span v-show="!parent">... }</span>
    </p>
    <p class="alpaca-p" v-show="parent" v-for="(item, index) in data" :key="index">
      <slot v-if="isObject(item)">
        <p @click="toggle(index)" class="alpaca-finger">
          {{ index }}: {
          <span v-show="!child[index]" v-if="isLast(index)">... }</span>
          <span v-show="!child[index]" v-else>... },</span>
        </p>
        <objectself v-show="child[index]" :data="item" :showLeaf="false"></objectself>
        <span v-show="child[index]" v-if="isLast(index)">}</span>
        <span v-show="child[index]" v-else>},</span>
      </slot>
      <slot v-else-if="isArray(item)">
        <p @click="toggle(index)" class="alpaca-finger">
          {{ index }}: [
          <span v-show="!child[index]" v-if="isLast(index)">... ]</span>
          <span v-show="!child[index]" v-else>... ],</span>
        </p>
        <arrayTree v-show="child[index]" :data="item" :showLeaf="false"></arrayTree>
        <span v-show="child[index]" v-if="isLast(index)">]</span>
        <span v-show="child[index]" v-else>],</span>
      </slot>
      <slot v-else>
        <span>{{ index }}:</span>
        <!-- 最后一项不需要逗号分隔 -->
        <span :class="getClass(item)" v-if="isLast(index)">{{ isNullOrUndefined(item) }}</span>
        <span :class="getClass(item)" v-else>{{ isNullOrUndefined(item) }},</span>
      </slot>
    </p>
    <p v-if="showLeaf" v-show="parent">}</p>
  </div>
</template>

<script>
import * as type from './typeof.js'
export default {
  name: 'objectself',
  components: {
    arrayTree: () => import('./arrayTree.vue')
  },
  props: {
    data: {
      type: Object,
      default: () => ({})
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
      let keys = Object.keys(this.data)
      keys.forEach(item => {
        if (type.isObject(this.data[item]) || type.isArray(this.data[item])) {
          this.$set(this.child, item, true)
        }
      })
    }
  },
  created () {
    let keys = Object.keys(this.data)
    keys.forEach(item => {
      if (type.isObject(this.data[item]) || type.isArray(this.data[item])) {
        this.$set(this.child, item, true)
      }
    })
  },
  methods: {
    isArray (item) {
      return type.isArray(item)
    },
    isObject (item, index) {
      return type.isObject(item)
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
    isLast (key) {
      let allKeys = Object.keys(this.data)
      return allKeys[allKeys.length - 1] === key
    },
    toggle (index) {
      this.child[index] = !this.child[index]
    }
  }
}
</script>
