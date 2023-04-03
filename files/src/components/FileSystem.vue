<template>
  <div>
    <div class="path-navigation">
      <button @click="goBack" :disabled="!navHistory.length">Back</button>
      <span>{{ currentPath }}</span>
    </div>
    <div class="file-system-container">
      <div class="file-system">
        <file-item v-for="item in currentFolder.children" :key="item.name" :item="item" @click="onItemClick"
          @open-folder="openFolder"></file-item>
      </div>
      <file-viewer :file="selectedFile"></file-viewer>
    </div>
  </div>
</template>

<script>
import FileItem from '@/components/FileItem.vue';
import FileViewer from '@/components/FileViewer.vue';

export default {
  components: {
    FileItem,
    FileViewer,
  },
  data() {
    return {
      fileSystem: [
        {
          name: 'Folder1',
          type: 'folder',
          children: [
            { name: 'File1.txt', type: 'file', content: 'This is File1.txt content.' },
            { name: 'File2.txt', type: 'file', content: 'This is File2.txt content.' },
            {
              name: 'Subfolder1',
              type: 'folder',
              children: [
                { name: 'File3.txt', type: 'file', content: 'This is File3.txt content.' },
              ],
            },
          ],
        },
        {
          name: 'Folder2',
          type: 'folder',
          children: [
            { name: 'File4.txt', type: 'file', content: 'This is File4.txt content.' },
            { name: 'File5.txt', type: 'file', content: 'This is File5.txt content.' },
          ],
        },
        { name: 'File6.txt', type: 'file', content: 'This is File6.txt content.' },
      ],
      navHistory: [],
      currentFolder: { children: [] },
      selectedFile: null,
    };
  },
  created() {
    this.currentFolder.children = this.fileSystem;
  },
  computed: {
    currentPath() {
      return '/' + this.navHistory.join('/');
    },
  },
  methods: {
    onItemClick(item) {
      if (item.type === 'file') {
        this.selectedFile = item;
      } else {
        this.selectedFile = null;
      }
    },
    openFolder(folder) {
      this.navHistory.push(folder.name);
      this.currentFolder = folder;
      this.selectedFile = null;
    },
    goBack() {
      this.navHistory.pop();
      this.currentFolder = this.navHistory.reduce(
        (folder, name) => {
          const nextFolder = folder.children.find((item) => item.name === name);
          return nextFolder || folder;
        },
        { children: this.fileSystem },
      );
      this.selectedFile = null;
    },
  },
};
</script>
