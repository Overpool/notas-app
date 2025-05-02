<template>
  <div class="modal">
    <div class="modal-content">
      <h2>Confirmar eliminación</h2>
      <p class="confirmation-message">
        ¿Estás seguro que deseas eliminar la nota "<strong>{{ note.title }}</strong>"?
      </p>
      <div class="modal-actions">
        <button @click="$emit('close')">Cancelar</button>
        <button class="delete-button" @click="$emit('confirm')">Eliminar</button>
      </div>
    </div>
  </div>
</template>
  
<script>
export default {
  name: 'DeleteConfirmationModal',
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
.confirmation-message {
  margin-bottom: 20px;
}
.modal-actions {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
}
.modal-actions button {
  padding: 8px 15px;
  border: 1px solid #000;
  background-color: #e7e7e7;
  border-radius: 7px;
  cursor: pointer;
}

.modal-actions .delete-button {
  background-color: #ffcccc;
  border: 2px solid #ff0000;
}
.modal-actions .delete-button:hover {
  background-color: #ff0000;
  color: #fff;
}
.modal-actions button:hover {
  background-color: #000;
  color: #fff;
}


</style>
