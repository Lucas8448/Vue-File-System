<template>
  <div class="file-item" @click="onClick" @dblclick="onDoubleClick">
    <span :class="item.type === 'folder' ? 'folder-icon' : 'file-icon'">
      {{ item.type === 'folder' ? '📁' : '📄' }}
    </span>
    <span>{{ item.name }}</span>
  </div>
</template>

<script>
export default {
  props: {
    item: Object,
  },
  methods: {
    onClick() {
      this.$emit('click', this.item);
    },
    onDoubleClick() {
      if (this.item.type === 'folder') {
        this.$emit('open-folder', this.item);
      } else if (this.item.type === 'file') {
        this.$emit('click', this.item);
      }
    },
  },
};
</script>