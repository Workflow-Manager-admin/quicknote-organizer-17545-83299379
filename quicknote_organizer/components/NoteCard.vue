<script setup>
const props = defineProps({
  note: {
    type: Object,
    required: true
  }
});

const emit = defineEmits(['edit', 'delete']);
</script>

<template>
  <div class="note-card">
    <div class="note-header">
      <h3 class="note-title">{{ note.title }}</h3>
      <div class="note-actions">
        <button class="action-btn edit" @click="emit('edit', note)">
          Edit
        </button>
        <button class="action-btn delete" @click="emit('delete', note.id)">
          Delete
        </button>
      </div>
    </div>
    <div class="note-content">{{ note.content }}</div>
    <div class="note-category" :style="{ backgroundColor: note.color }">
      {{ note.category }}
    </div>
  </div>
</template>

<style scoped>
.note-card {
  background: white;
  border-radius: 8px;
  padding: 16px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
  position: relative;
}

.note-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
}

.note-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 12px;
}

.note-title {
  margin: 0;
  font-size: 18px;
  color: #333;
  word-break: break-word;
}

.note-actions {
  display: flex;
  gap: 8px;
}

.action-btn {
  padding: 4px 8px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 12px;
  transition: all 0.2s ease;
}

.action-btn.edit {
  background-color: #FFC107;
  color: #000;
}

.action-btn.delete {
  background-color: #dc3545;
  color: white;
}

.action-btn:hover {
  opacity: 0.8;
}

.note-content {
  color: #666;
  font-size: 14px;
  line-height: 1.4;
  margin-bottom: 12px;
  word-break: break-word;
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.note-category {
  display: inline-block;
  padding: 4px 8px;
  border-radius: 12px;
  color: white;
  font-size: 12px;
  font-weight: 500;
}
</style>
