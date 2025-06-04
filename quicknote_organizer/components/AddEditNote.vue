<script setup>
import { ref, onMounted } from 'vue';

const props = defineProps({
  note: {
    type: Object,
    default: null
  }
});

const emit = defineEmits(['save', 'close']);

const title = ref('');
const content = ref('');
const category = ref('');
const color = ref('#1976D2');

const categories = [
  { name: 'General', color: '#1976D2' },
  { name: 'Work', color: '#2E7D32' },
  { name: 'Personal', color: '#C2185B' },
  { name: 'Ideas', color: '#7B1FA2' },
  { name: 'Tasks', color: '#F57C00' }
];

onMounted(() => {
  if (props.note) {
    title.value = props.note.title;
    content.value = props.note.content;
    category.value = props.note.category;
    color.value = props.note.color;
  }
});

const handleSave = () => {
  if (!title.value.trim() || !content.value.trim() || !category.value) {
    return;
  }

  const noteData = {
    title: title.value.trim(),
    content: content.value.trim(),
    category: category.value,
    color: color.value
  };

  if (props.note) {
    noteData.id = props.note.id;
  }

  emit('save', noteData);
};

const handleCategorySelect = (cat) => {
  category.value = cat.name;
  color.value = cat.color;
};
</script>

<template>
  <div class="modal-overlay" @click="emit('close')">
    <div class="modal-content" @click.stop>
      <h2>{{ props.note ? 'Edit Note' : 'Add New Note' }}</h2>
      
      <div class="form-group">
        <input
          v-model="title"
          type="text"
          placeholder="Title"
          class="form-input"
        />
      </div>

      <div class="form-group">
        <textarea
          v-model="content"
          placeholder="Note content..."
          class="form-input"
          rows="4"
        ></textarea>
      </div>

      <div class="category-selector">
        <label>Category:</label>
        <div class="category-buttons">
          <button
            v-for="cat in categories"
            :key="cat.name"
            class="category-btn"
            :class="{ active: category === cat.name }"
            :style="{ backgroundColor: cat.color }"
            @click="handleCategorySelect(cat)"
          >
            {{ cat.name }}
          </button>
        </div>
      </div>

      <div class="modal-actions">
        <button class="btn cancel" @click="emit('close')">Cancel</button>
        <button class="btn save" @click="handleSave">Save</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background-color: white;
  padding: 24px;
  border-radius: 8px;
  width: 90%;
  max-width: 500px;
  max-height: 90vh;
  overflow-y: auto;
}

h2 {
  margin: 0 0 20px 0;
  color: #333;
}

.form-group {
  margin-bottom: 16px;
}

.form-input {
  width: 100%;
  padding: 8px 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
}

textarea.form-input {
  resize: vertical;
  min-height: 100px;
}

.category-selector {
  margin-bottom: 20px;
}

.category-selector label {
  display: block;
  margin-bottom: 8px;
  color: #666;
}

.category-buttons {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

.category-btn {
  padding: 6px 12px;
  border: none;
  border-radius: 16px;
  color: white;
  cursor: pointer;
  font-size: 14px;
  opacity: 0.7;
  transition: all 0.2s ease;
}

.category-btn:hover,
.category-btn.active {
  opacity: 1;
}

.modal-actions {
  display: flex;
  justify-content: flex-end;
  gap: 12px;
  margin-top: 24px;
}

.btn {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  transition: all 0.2s ease;
}

.btn.cancel {
  background-color: #f5f5f5;
  color: #333;
}

.btn.save {
  background-color: #1976D2;
  color: white;
}

.btn:hover {
  opacity: 0.9;
}
</style>
