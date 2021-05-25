<template>
  <div class="line-clamp" :style="{backgroundColor:bgColor}">
    <input type="checkbox" class="line-clamp-checkbox" :id="id" />
    <div class="line-clamp-content" :style="{maxHeight:maxHeight}">
      <label :for="id" class="line-clamp-btn line-clamp-btn-open">
        <span :style="{color:btn.color}">{{btn.open}}</span>
      </label>
      <slot></slot>
      <label :for="id" class="line-clamp-btn line-clamp-btn-close">
        <span :style="{color:btn.color}">{{btn.close}}</span>
      </label>
    </div>
  </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue'
export default defineComponent({
  name: 'lineClamp',
  props: {
    bgColor: {
      default: '#fff',
    },
    line: {
      default: 2,
      type: Number,
    },
    btn: {
      default: () => ({
        color: 'red',
        open: '更多',
        close: '收起',
      }),
    },
  },
  methods: {},
  computed: {
    maxHeight(): string {
      return this.line * 1.5 + 'em'
    },
    id() {
      return 'line-clamp-' + new Date().getTime() + Math.floor(Math.random() * 100)
    },
  },
})
</script>
<style lang="scss" scoped>
.line-clamp {
  display: flex;
  &-checkbox {
    display: none;
  }

  &-content {
    line-height: 1.5;
    position: relative;
    overflow: hidden;
    text-overflow: ellipsis;
    text-align: justify;
    background-color: inherit;
    &::before {
      float: right;
      height: calc(100% - 1.436em);
      content: '';
    }
    &::after {
      position: absolute;
      width: 100%;
      content: '';
      background-color: inherit;
      height: 2em;
    }
  }
  &-btn {
    float: right;
    clear: both;
    margin-left: 0.5em;
    &-open {
      &::before {
        content: '...';
        transform: translate(-0.5em, 0);
        display: inline-block;
      }
    }
    &-close {
      display: none;
      margin-left: 0.5em;
    }
  }
  &-checkbox:checked + &-content {
    max-height: inherit !important;
    &::before {
      display: none;
    }
    &::after {
      visibility: hidden;
    }
  }
  &-checkbox:checked + &-content > &-btn {
    &-open {
      display: none;
    }
    &-close {
      display: inline;
    }
  }
}
</style>
