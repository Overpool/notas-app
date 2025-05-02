<template>
  <div id="app">
    <h1>NOTAS</h1>
    
    <div class="input-container">
      <input type="text" v-model="searchQuery" placeholder="Buscar notas...">
      <button @click="openAddNoteModal">AGREGAR NOTA</button>
    </div>
    
    <div v-if="filteredNotes.length === 0" class="empty-state">
      <p>No hay notas disponibles</p>
    </div>
    
    <div class="notes-grid">
      <NoteCard 
        v-for="note in filteredNotes" 
        :key="note.id" 
        :note="note"
        @view="openViewNoteModal"
        @edit="openEditNoteModal"
        @delete="deleteNote"
      />
    </div>
    
    <!-- Add Note Modal -->
    <AddNoteModal 
      v-if="showAddModal"
      @close="closeModal"
      @save="createNote"
      v-model:note="newNote"
    />
    
    <!-- Edit Note Modal -->
    <EditNoteModal
      v-if="showEditModal"
      :note="currentNote"
      @close="closeModal"
      @update="updateNote"
    />
    
    <!-- View Note Modal -->
    <ViewNoteModal
      v-if="showViewModal"
      :note="currentNote"
      @close="closeModal"
      @edit="openEditNoteModal"
    />

  </div>
</template>

<script>
import { ref, onMounted, computed } from 'vue';
import axios from 'axios';
import NoteCard from './components/NoteCard.vue';
import AddNoteModal from './components/AddNoteModal.vue';
import EditNoteModal from './components/EditNoteModal.vue';
import ViewNoteModal from './components/ViewNoteModal.vue';

export default {
  name: 'App',
  components: {
    NoteCard,
    AddNoteModal,
    EditNoteModal,
    ViewNoteModal
  },
  setup() {
    const notes = ref([]);
    const searchQuery = ref('');
    const apiUrl = 'https://notesapp-cqgdh8erbphragb0.canadacentral-01.azurewebsites.net'; // Azure hosted API
    
    const showAddModal = ref(false);
    const showEditModal = ref(false);
    const showViewModal = ref(false);
    
    const newNote = ref({
      title: '',
      content: ''
    });
    
    const currentNote = ref({
      id: null,
      title: '',
      content: '',
      created_at: null,
      updated_at: null
    });
    
    const filteredNotes = computed(() => {
      if (!searchQuery.value) return notes.value;
      
      const query = searchQuery.value.toLowerCase();
      return notes.value.filter(note => 
        note.title.toLowerCase().includes(query) || 
        note.content.toLowerCase().includes(query)
      );
    });
    
    const fetchNotes = async () => {
      try {
        const response = await axios.get(`${apiUrl}/notes/`);
        notes.value = response.data;
      } catch (error) {
        console.error('Error fetching notes:', error);
        alert('Error al obtener las notas');
      }
    };
    
    const createNote = async () => {
      if (!newNote.value.title || !newNote.value.content) {
        alert('Por favor complete todos los campos');
        return;
      }
      
      try {
        await axios.post(`${apiUrl}/notes/`, newNote.value);
        await fetchNotes();
        closeModal();
        newNote.value = { title: '', content: '' };
      } catch (error) {
        console.error('Error creating note:', error);
        alert('Error al crear la nota');
      }
    };
    
    const updateNote = async (updatedNote) => {
      if (!updatedNote.title || !updatedNote.content) {
        alert('Por favor complete todos los campos');
        return;
      }

      try {
        await axios.put(`${apiUrl}/notes/${updatedNote.id}`, {
          title: updatedNote.title,
          content: updatedNote.content
        });
        await fetchNotes();
        closeModal();
      } catch (error) {
        console.error('Error updating note:', error);
        alert('Error al actualizar la nota');
      }
    };

    const deleteNote = async (id) => {
      if (!confirm('¿Está seguro que desea eliminar esta nota?')) return;
      
      try {
        await axios.delete(`${apiUrl}/notes/${id}`);
        await fetchNotes();
      } catch (error) {
        console.error('Error deleting note:', error);
        alert('Error al eliminar la nota');
      }
    };
  
    const openAddNoteModal = () => {
      showAddModal.value = true;
      newNote.value = { title: '', content: '' };
    };
    
    const openEditNoteModal = (note) => {
      currentNote.value = { ...note };
      showViewModal.value = false;
      showEditModal.value = true;
    };
    
    const openViewNoteModal = (note) => {
      currentNote.value = { ...note };
      showViewModal.value = true;
    };
    
    const closeModal = () => {
      showAddModal.value = false;
      showEditModal.value = false;
      showViewModal.value = false;
    };
    
    onMounted(() => {
      fetchNotes();
    });
    
    return {
      notes,
      searchQuery,
      filteredNotes,
      showAddModal,
      showEditModal,
      showViewModal,
      newNote,
      currentNote,
      fetchNotes,
      createNote,
      updateNote,
      deleteNote,
      openAddNoteModal,
      openEditNoteModal,
      openViewNoteModal,
      closeModal
    };
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Arial, sans-serif;
  
}
body {
  padding: 20px;
  max-width: 1200px;
  margin: 0 auto;
  font-size: 18px;
}
h1 {
  text-align: center;
  margin-bottom: 30px;
}
.input-container {
  display: flex;
  gap: 10px;
  margin-bottom: 30px;
}
.input-container input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 7px;
}
.input-container button {
  padding: 10px 15px;
  background-color: #e7e7e7;
  border: 1px solid #000;
  border-radius: 7px;
  cursor: pointer;
}
.input-container button:hover {
  background-color: #000;
  color: #fff;
}

.notes-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
}
.empty-state {
  text-align: center;
  margin-top: 50px;
  color: #666;
}
</style>