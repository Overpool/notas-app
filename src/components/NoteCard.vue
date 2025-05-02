<template>
  <div class="note" @click="$emit('view', note)">
    <h3>{{ note.title }}</h3>
    <p v-if="note.content.length > 100">{{ note.content.substring(0, 100) }}...</p>
    <p v-else>{{ note.content }}</p>
    <div class="created-at">
      {{ formatDate(note.created_at) }}
    </div>
    <div class="note-actions" @click.stop>
      <button @click="$emit('edit', note)">Editar</button>
      <button class="delete" @click="$emit('delete', note.id)">Eliminar</button>

    </div>
  </div>
</template>

<script>
export default {
  name: 'NoteCard',
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
.note {
  border: 1px solid #ffffff;
  border-radius: 7px;
  box-shadow:0 0 10px rgba(0, 0, 0, 0.5);
  background-color: #f9f9f9;
  padding: 15px;
  min-height: 100px;
  position: relative;
  cursor: pointer;
}
.note h3 {
  margin-bottom: 10px;
  word-break: break-word;
}
.note p {
  word-break: break-word;
}
.note-actions {
  position: absolute;
  bottom: 10px;
  right: 10px;
  display: flex;
  gap: 5px;
}
.note-actions button {
  padding: 5px 10px;
  background-color: #e7e7e7;
  border: 2px solid #000;
  cursor: pointer;
  border-radius: 7px;
}

.note-actions .delete {
  background-color: #ffcccc;
  border: 2px solid #ff0000;
}
.note-actions .delete:hover {
  background-color: #ff0000;
  color: #fff;
}
.note-actions button:hover {
  background-color: #000;
  color: #fff;
}

.created-at {
  font-size: 0.8em;
  color: #666;
  margin-top: 15px;
}
</style>