<template>
  <div class="modal">
    <div class="modal-content">
      <h2>{{ note.title }}</h2>
      <p class="note-content">{{ note.content }}</p>
      <div class="created-at">
        Creado: {{ formatDate(note.created_at) }}
        <br>
        Actualizado: {{ formatDate(note.updated_at) }}
      </div>
      <div class="modal-actions">
        <button class="close" @click="$emit('close')">Cerrar</button>
        <button class="edit" @click="$emit('edit', note)">Editar</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ViewNoteModal',
  props: {
    note: {
      type: Object,
      required: true
    }
  },
  methods: {
    formatDate(dateString) {
      if (!dateString) return '';
      const date = new Date(dateString);
      return date.toLocaleString();
    }
  }
}
</script>

<style scoped>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}
.modal-content {
  background-color: #fff;
  padding: 20px;
  border-radius: 5px;
  width: 500px;
  max-width: 90%;
}
.modal h2 {
  margin-bottom: 15px;
}
.note-content {
  white-space: pre-line;
  margin-bottom: 20px;
}
.created-at {
  font-size: 0.8em;
  color: #666;
  margin-top: 15px;
  margin-bottom: 15px;
}
.modal-actions {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
}
.modal-actions button {
  padding: 8px 15px;
  border: 1px solid #000;
  background-color: #fff;
  border-radius: 7px;
  cursor: pointer;
}

.modal-actions .edit {
  background-color: #99faa8;
  color: #000;
}
.modal-actions .edit:hover {
  background-color: #22eb3a;
  color: #fff;
}
.modal-actions .close {
  background-color: #ffcccc;
  color: #000;
}
.modal-actions .close:hover {
  background-color: #ff0000;
  color: #fff;
}
</style>