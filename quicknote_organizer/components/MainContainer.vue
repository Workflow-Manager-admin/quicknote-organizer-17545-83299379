<script setup>
import { ref, computed, onMounted } from 'vue';
import NoteCard from './NoteCard.vue';
import AddEditNote from './AddEditNote.vue';

// State management with Vue composition API
const searchQuery = ref('');
const showAddEditModal = ref(false);
const selectedNote = ref(null);
const notes = ref([]);

// Initialize with a welcome note
onMounted(() => {
  if (notes.value.length === 0) {
    notes.value = [{
      id: 1,
      title: 'Welcome to QuickNote',
      content: 'This is your first note. Try adding more notes using the + button!',
      category: 'General',
      color: '#1976D2',
      createdAt: new Date().toISOString()
    }];
  }
});

const filteredNotes = computed(() => {
  const query = searchQuery.value.toLowerCase().trim();
  if (!query) return notes.value;
  
  return notes.value.filter(note => 
    note.title.toLowerCase().includes(query) || 
    note.content.toLowerCase().includes(query) ||
    note.category.toLowerCase().includes(query)
  );
});

const addNote = (noteData) => {
  const newNote = {
    id: Date.now(),
    createdAt: new Date().toISOString(),
    ...noteData
  };
  notes.value = [newNote, ...notes.value];
  showAddEditModal.value = false;
};

const editNote = (noteData) => {
  const index = notes.value.findIndex(n => n.id === noteData.id);
  if (index !== -1) {
    notes.value[index] = { 
      ...notes.value[index], 
      ...noteData,
      updatedAt: new Date().toISOString()
    };
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
      <TransitionGroup name="note-list">
        <NoteCard
          v-for="note in filteredNotes"
          :key="note.id"
          :note="note"
          @edit="openEditNote"
          @delete="deleteNote"
        />
      </TransitionGroup>
    </div>

    <!-- Floating Action Button -->
    <button class="fab" @click="openAddNote" title="Add new note">
      <span class="plus">+</span>
    </button>

    <!-- Add/Edit Modal -->
    <Transition name="modal">
      <AddEditNote
        v-if="showAddEditModal"
        :note="selectedNote"
        @save="selectedNote ? editNote : addNote"
        @close="showAddEditModal = false"
      />
    </Transition>
  </div>
</template>

<style scoped>
.main-container {
  padding: 20px;
  height: 100%;
  position: relative;
  max-width: 1440px;
  margin: 0 auto;
}

.search-bar {
  margin-bottom: 20px;
  max-width: 600px;
  margin: 0 auto 20px;
}

.search-input {
  width: 100%;
  padding: 12px 20px;
  border: 2px solid #1976D2;
  border-radius: 25px;
  font-size: 16px;
  outline: none;
  transition: all 0.3s ease;
  background-color: white;
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
  z-index: 100;
}

.fab:hover {
  background-color: #1565C0;
  transform: scale(1.05);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.plus {
  font-size: 24px;
  font-weight: bold;
}

/* Transitions */
.note-list-enter-active,
.note-list-leave-active {
  transition: all 0.3s ease;
}

.note-list-enter-from,
.note-list-leave-to {
  opacity: 0;
  transform: translateY(30px);
}

.modal-enter-active,
.modal-leave-active {
  transition: opacity 0.3s ease;
}

.modal-enter-from,
.modal-leave-to {
  opacity: 0;
}

@media (max-width: 768px) {
  .main-container {
    padding: 10px;
  }
  
  .notes-grid {
    grid-template-columns: 1fr;
  }
  
  .fab {
    bottom: 20px;
    right: 20px;
  }
}
</style>
