<template>
  <div class="file-viewer" v-if="file">
    <h2>{{ file.name }}</h2>
    <pre>{{ file.content }}</pre>
  </div>
</template>

<script>
export default {
  props: {
    file: Object,
  },
};
</script>