<script setup>
import { ref } from 'vue'
import Modal from './components/Modal.vue'
import FolderTree from './components/FolderTree.vue'

const isModalOpen = ref(false)
const selectedFolderId = ref(null)

const mockFolders = [
  { 
    id: 1, 
    name: 'Папка 1', 
    children: [
      { id: 2, name: 'Папка 1.1', children: [] },
      { 
        id: 3, 
        name: 'Папка 1.2', 
        children: [
          { id: 4, name: 'Папка 1.2.1', children: [] }
        ]
      }
    ]
  },
  { id: 5, name: 'Папка 2', children: [] },
]

const handleSelect = (folderId) => {
  selectedFolderId.value = folderId
  isModalOpen.value = false
  console.log('Selected folder ID:', folderId)
}
</script>

<template>
  <div class="app">
    <button @click="isModalOpen = true" class="open-button">Открыть</button>
    
    <Modal 
      v-if="isModalOpen" 
      title="Выберите папку"
      @close="isModalOpen = false"
    >
      <FolderTree 
        :folders="mockFolders"
        @select="handleSelect"
      />
    </Modal>
  </div>
</template>

<style>
.app {
  padding: 20px;
}

.open-button {
  padding: 8px 16px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

.open-button:hover {
  background-color: #45a049;
}
</style>