<script setup>
import { ref } from 'vue'

const props = defineProps({
  folders: {
    type: Array,
    required: true
  }
})

const emit = defineEmits(['select'])

const expandedFolders = ref(new Set())
const selectedFolder = ref(null)

const toggleFolder = (folderId) => {
  if (expandedFolders.value.has(folderId)) {
    expandedFolders.value.delete(folderId)
  } else {
    expandedFolders.value.add(folderId)
  }
}

const selectFolder = (folder) => {
  selectedFolder.value = folder.id
}

const handleOk = () => {
  if (selectedFolder.value) {
    emit('select', selectedFolder.value)
  }
}
</script>

<template>
  <div class="folder-tree">
    <div class="tree-content">
      <div v-for="folder in folders" :key="folder.id" class="folder-item">
        <div 
          class="folder-row"
          :class="{ 'selected': selectedFolder === folder.id }"
          @click="selectFolder(folder)"
        >
          <span 
            v-if="folder.children.length > 0"
            class="toggle-icon"
            @click.stop="toggleFolder(folder.id)"
          >
            {{ expandedFolders.has(folder.id) ? '▼' : '▶' }}
          </span>
          <span class="folder-icon">📁</span>
          <span class="folder-name">{{ folder.name }}</span>
        </div>
        
        <div 
          v-if="folder.children.length > 0 && expandedFolders.has(folder.id)"
          class="folder-children"
        >
          <FolderTree 
            :folders="folder.children"
            @select="emit('select', $event)"
          />
        </div>
      </div>
    </div>
    
    <div class="tree-footer">
      <button 
        class="button ok"
        :disabled="!selectedFolder"
        @click="handleOk"
      >
        Ок
      </button>
    </div>
  </div>
</template>

<style scoped>
.folder-tree {
  min-width: 250px;
}

.tree-content {
  margin-bottom: 20px;
}

.folder-item {
  margin: 4px 0;
}

.folder-row {
  display: flex;
  align-items: center;
  padding: 4px 8px;
  cursor: pointer;
  border-radius: 4px;
}

.folder-row:hover {
  background-color: #f5f5f5;
}

.folder-row.selected {
  background-color: #e3f2fd;
}

.toggle-icon {
  margin-right: 4px;
  font-size: 12px;
  cursor: pointer;
  width: 20px;
  text-align: center;
}

.folder-icon {
  margin-right: 8px;
}

.folder-children {
  margin-left: 24px;
}

.tree-footer {
  display: flex;
  justify-content: flex-end;
}

.button {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.ok {
  background-color: #4CAF50;
  color: white;
}

.ok:hover:not(:disabled) {
  background-color: #45a049;
}
</style>