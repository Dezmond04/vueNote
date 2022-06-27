<template>
  <!-- new note -->
  <div class="new-note">
    <label>Title</label>
    <input v-model="note.title" type="text" />
    <div class="priority">
      <label>
        <span>Стандартный</span>

        <input type="radio" name="priority" value="1" />
      </label>
      <label>
        <span>Важно</span>
        <input type="radio" name="priority" value="2" />
      </label>
      <label>
        <span>Очень важно</span>

        <input type="radio" name="priority" value="3" />
      </label>
    </div>
    <label>Description</label>
    <textarea v-model="note.descr"></textarea>
    <button class="btn btnPrimary" @click="addNote">New Note</button>
  </div>
</template>

<script>
export default {
  props: {
    note: {
      type: Object,
      required: true,
    },
  },
  methods: {
    addNote() {
      this.$emit("addNote", this.note);
      //$emit - благодаря ему мы может отправлять данные обратно родителю, обязательный параметр, название текушего эмита, может не совпадать с названием метода, и пторой не обязательный параметр - то что мы возвращаем обратно родителю
    },
  },
};
</script>

<style lang="scss">
.new-note {
  text-align: center;
}
.priority {
  display: flex;
  width: 60%;
  margin: 20px auto;
  justify-content: space-between;
  align-items: center;
}
</style>
