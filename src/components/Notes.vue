<template>
  <!-- note list -->
  <div class="notes">
    <div
      class="note"
      :class="{
        full: !grid,
        middle: note.priority == 2,
        high: note.priority == 3,
      }"
      v-for="(note, index) in notes"
      :key="index"
    >
      <div class="note-header" :class="{ full: !grid }">
        <p class="note-header__title" @click="renameTitle(index)">
          <span>{{ note.title }}</span>
          <input v-model="note.title" type="text" />
        </p>
        <p style="cursor: pointer" @click="removeNote(index)">x</p>
      </div>
      <div class="note-body">
        <p>{{ note.descr }}</p>
        <span> {{ note.date }} </span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    notes: {
      type: Array,
      required: true,
    },
    grid: {
      type: Boolean,
      required: true,
    },
  },
  methods: {
    removeNote(index) {
      this.$emit("remove", index);
    },
    renameTitle(index) {
      this.$emit("renameTitle", index);
    },
  },
};
</script>

<style lang="scss">
.notes {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  padding: 40px 0;
}
.note {
  width: 48%;
  padding: 18px 20px;
  margin-bottom: 20px;
  background-color: #fff;
  transition: all 0.25s cubic-bezier(0.02, 0.01, 0.47, 1);
  box-shadow: 0 30px 30px rgba(0, 0, 0, 0.02);
  &:hover {
    box-shadow: 0 30px 30px rgba(0, 0, 0, 0.04);
    transform: translate(0, -6px);
    transition-delay: 0s !important;
  }
  &.full {
    width: 100%;
    text-align: center;
  }
}

.note-header {
  display: flex;
  justify-content: space-between;
  align-items: center;

  &.full {
    justify-content: center;
    p {
      margin-right: 16px;
      &:last-child {
        margin-right: 0;
      }
    }
  }
}
.note-header__title {
  position: relative;
  width: 100%;
  margin-right: 15px;
  & input {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: none;
    font-size: 22px;
    color: #402caf;
    &.show {
      display: block;
    }
  }
  & span {
    font-size: 22px;
    color: #402caf;
  }
}
.middle {
  background-color: #ff9800;
  color: #fff;
  & .note-header__title span {
    color: #081cc1;
  }
}
.high {
  background-color: #f44336;
  color: #fff;
  & .note-header__title span {
    color: #000;
  }
}
.note-body {
  p {
    margin: 20px 0;
  }
  span {
    font-size: 14px;
    color: #999999;
  }
}
svg {
  color: #999999;
  cursor: pointer;
  &.active {
    color: #402caf;
  }
  &:not(:last-child) {
    margin-right: 12px;
  }
}
</style>
