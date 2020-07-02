<template>
  <div>
    <p class="alpaca-p" v-for="(item, index) in data" :key="index" :style="getStyle">
      <span class="alpaca-line">{{ item.line }}</span>
      <slot v-if="isObject(item.type)">
        <p @click="toggle(item)" class="alpaca-f">
          <span v-if="needKey" class="alpaca-k">"{{ item.key }}":</span>
          <span class="alpaca-k">{</span>
          <span class="alpaca-k" v-show="!item.show">... } {{ comma(index) }}</span>
        </p>
        <self
          v-show="item.show"
          :data="item.value"
          :showLeaf="false"
          :indent="indent+1"
          :needKey="true"
        ></self>
        <p v-show="item.show" :style="getStyle" class="alpaca-p">
          <span class="alpaca-line">{{ item.lastLine }}</span>
          <span class="alpaca-k">} {{ comma(index) }}</span>
        </p>
      </slot>
      <slot v-else-if="isArray(item.type)">
        <p @click="toggle(item)" class="alpaca-f">
          <span v-if="needKey" class="alpaca-k">"{{ item.key }}":</span>
          <span class="alpaca-k">[</span>
          <span class="alpaca-k" v-show="!item.show">... ] {{ comma(index) }}</span>
        </p>
        <self
          v-show="item.show"
          :data="item.value"
          :showLeaf="false"
          :indent="indent+1"
          :needKey="false"
        ></self>
        <p v-show="item.show" :style="getStyle" class="alpaca-p">
          <span class="alpaca-line">{{ item.lastLine }}</span>
          <span class="alpaca-k">] {{ comma(index) }}</span>
        </p>
      </slot>
      <slot v-else>
        <span v-if="needKey" class="alpaca-k">"{{ item.key }}":</span>
        <span :class="getClass(item.type)">{{ item.value }}{{ comma(index) }}</span>
      </slot>
    </p>
  </div>
</template>

<script>
export default {
  name: "self",
  props: {
    data: {
      default: () => []
    },
    showLeaf: {
      default: true
    },
    indent: {
      default: 1
    },
    needKey: {
      default: true
    }
  },
  computed: {
    getStyle() {
      return { textIndent: `${(this.indent + 1) * 20}px` };
    }
  },
  methods: {
    notTree(type) {
      return type !== "Object" && type !== "Array";
    },
    toggle(item) {
      item.show = !item.show;
    },
    isObject(type) {
      return type === "Object";
    },
    isArray(type) {
      return type === "Array";
    },
    comma(index) {
      if (index === this.data.length - 1) {
        return "";
      }
      return ",";
    },
    getClass(type) {
      if (type === "Number") {
        return "alpaca-number";
      }
      if (type === "String") {
        return "alpaca-string";
      }
      if (type === "Boolean") {
        return "alpaca-boolean";
      }
      if (type === "Undefined") {
        return "alpaca-undefined";
      }
      if (type === "Null") {
        return "alpaca-null";
      }
      // 其他不改变颜色
      return "";
    }
  }
};
</script>
