<script setup>
import { ref, computed } from 'vue';
import NoteCard from './NoteCard.vue';
import AddEditNote from './AddEditNote.vue';

const searchQuery = ref('');
const showAddEditModal = ref(false);
const selectedNote = ref(null);
const notes = ref([
  {
    id: 1,
    title: 'Welcome to QuickNote',
    content: 'This is your first note. Try adding more notes using the + button!',
    category: 'General',
    color: '#1976D2'
  }
]);

const filteredNotes = computed(() => {
  const query = searchQuery.value.toLowerCase();
  return notes.value.filter(note => 
    note.title.toLowerCase().includes(query) || 
    note.content.toLowerCase().includes(query) ||
    note.category.toLowerCase().includes(query)
  );
});

const addNote = (noteData) => {
  const newNote = {
    id: Date.now(),
    ...noteData
  };
  notes.value.unshift(newNote);
  showAddEditModal.value = false;
};

const editNote = (noteData) => {
  const index = notes.value.findIndex(n => n.id === noteData.id);
  if (index !== -1) {
    notes.value[index] = { ...notes.value[index], ...noteData };
  }
  showAddEditModal.value = false;
  selectedNote.value = null;
};

const deleteNote = (noteId) => {
  notes.value = notes.value.filter(note => note.id !== noteId);
};

const openAddNote = () => {
  selectedNote.value = null;
  showAddEditModal.value = true;
};

const openEditNote = (note) => {
  selectedNote.value = { ...note };
  showAddEditModal.value = true;
};
</script>

<template>
  <div class="main-container">
    <!-- Search Bar -->
    <div class="search-bar">
      <input 
        v-model="searchQuery"
        type="text"
        placeholder="Search notes..."
        class="search-input"
      />
    </div>

    <!-- Notes Grid -->
    <div class="notes-grid">
      <NoteCard
        v-for="note in filteredNotes"
        :key="note.id"
        :note="note"
        @edit="openEditNote"
        @delete="deleteNote"
      />
    </div>

    <!-- Floating Action Button -->
    <button class="fab" @click="openAddNote">
      <span class="plus">+</span>
    </button>

    <!-- Add/Edit Modal -->
    <AddEditNote
      v-if="showAddEditModal"
      :note="selectedNote"
      @save="selectedNote ? editNote : addNote"
      @close="showAddEditModal = false"
    />
  </div>
</template>

<style scoped>
.main-container {
  padding: 20px;
  height: 100%;
  position: relative;
}

.search-bar {
  margin-bottom: 20px;
}

.search-input {
  width: 100%;
  padding: 12px 20px;
  border: 2px solid #1976D2;
  border-radius: 25px;
  font-size: 16px;
  outline: none;
  transition: all 0.3s ease;
}

.search-input:focus {
  box-shadow: 0 0 5px rgba(25, 118, 210, 0.3);
}

.notes-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
  padding-bottom: 80px;
}

.fab {
  position: fixed;
  bottom: 30px;
  right: 30px;
  width: 56px;
  height: 56px;
  border-radius: 50%;
  background-color: #1976D2;
  color: white;
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16);
  transition: all 0.3s ease;
}

.fab:hover {
  background-color: #1565C0;
  transform: scale(1.05);
}

.plus {
  font-size: 24px;
  font-weight: bold;
}
</style>
