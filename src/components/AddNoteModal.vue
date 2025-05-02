<template>
  <div class="modal">
    <div class="modal-content">
      <h2>Agregar Nota</h2>
      <input type="text" v-model="note.title" placeholder="Título de la nota">
      <textarea v-model="note.content" placeholder="Contenido de la nota"></textarea>
      <div class="modal-actions">
        <button class="close" @click="$emit('close')">Cancelar</button>
        <button class="save" @click="$emit('save')">Guardar</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AddNoteModal',
  props: {
    note: {
      type: Object,
      required: true
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
.modal input, .modal textarea {
  width: 100%;
  padding: 10px;
  margin-bottom: 15px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
.modal textarea {
  min-height: 150px;
  resize: vertical;
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
  border-radius: 7PX;
  cursor: pointer;
}

.modal-actions .save {
  background-color: #99faa8;
  color: #000;
}
.modal-actions .save:hover {
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