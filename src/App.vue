<template>
  <div @click="closeInput" @keyup.enter="closeInputs" class="wrapper">
    <div class="wrapper-contet">
      <section>
        <div class="container">
          <message v-if="message" :message="message" />
          <!-- :message - байтим значение, что бы достать его из даты, без : будет считаться как просто строка -->

          <newNote :note="note" @addNote="addNote" />
          <!-- @addNote="addNote" 1. вызываем эмит которы будем использовать в данном случае эмит по клику (в дочернем компоненте описан) 2. вызываем метод который описан в родителе -->
          <div class="note-header" style="margin: 36px 0">
            <h1 class="title">{{ title }}</h1>
            <!-- search -->
            <search
              :value="search"
              placeholder="Find your note"
              @search="search = $event"
            />
            <!-- icons controls -->
            <div class="icons">
              <svg
                :class="{ active: grid }"
                @click="grid = true"
                xmlns="http://www.w3.org/2000/svg"
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
              >
                <rect x="3" y="3" width="7" height="7"></rect>
                <rect x="14" y="3" width="7" height="7"></rect>
                <rect x="14" y="14" width="7" height="7"></rect>
                <rect x="3" y="14" width="7" height="7"></rect>
              </svg>
              <svg
                :class="{ active: !grid }"
                @click="grid = false"
                xmlns="http://www.w3.org/2000/svg"
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
              >
                <line x1="8" y1="6" x2="21" y2="6"></line>
                <line x1="8" y1="12" x2="21" y2="12"></line>
                <line x1="8" y1="18" x2="21" y2="18"></line>
                <line x1="3" y1="6" x2="3" y2="6"></line>
                <line x1="3" y1="12" x2="3" y2="12"></line>
                <line x1="3" y1="18" x2="3" y2="18"></line>
              </svg>
            </div>
          </div>
          <!-- radio -->

          <!-- note list -->
          <notes
            :notes="notesFilter"
            :grid="grid"
            @remove="removeNote"
            @renameTitle="renameTitle"
          />
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import message from "@/components/Message.vue";
import newNote from "@/components/NewNote.vue";
import notes from "@/components/Notes.vue";
import search from "@/components/Search.vue";

export default {
  components: {
    message,
    newNote,
    notes,
    search,
  },
  data() {
    return {
      title: "Notes App",
      search: "",
      message: null,
      grid: true,
      note: {
        title: "",
        priority: 1,
        descr: "",
      },
      notes: [
        {
          title: "First Note",
          priority: "low",
          descr: "Description for first note",
          date: new Date(Date.now()).toLocaleString(),
        },
        {
          title: "Second Note",
          priority: 1,
          descr: "Description for second note",
          date: new Date(Date.now()).toLocaleString(),
        },
        {
          title: "Third Note",
          priority: 1,
          descr: "Description for third note",
          date: new Date(Date.now()).toLocaleString(),
        },
      ],
    };
  },
  computed: {
    notesFilter() {
      let array = this.notes;
      let search = this.search;

      if (!search) return array;
      //Small
      search = search.trim().toLowerCase();
      //Filter
      array = array.filter(function (item) {
        if (item.title.toLowerCase().indexOf(search) !== -1) {
          return item;
        }
      });
      //Error
      return array;
    },
  },
  methods: {
    addNote() {
      // console.log(this.note);
      let { title, priority, descr } = this.note;
      let priorityInput = document.querySelectorAll(".priority input");

      priorityInput.forEach((radio) => {
        if (radio.checked) {
          priority = radio.value;
        }
      });
      if (title === "") {
        this.message = "Title can`t be blank";
        return false;
      }

      this.notes.push({
        title,
        priority,
        descr,
        date: new Date(Date.now()).toLocaleString(),
      });
      console.log(this.notes);
      this.message = null;
      this.note.title = "";
      this.note.descr = "";
      priorityInput.forEach((input) => {
        input.checked = false;
      });
    },
    removeNote(index) {
      this.notes.splice(index, 1);
    },
    renameTitle(index) {
      const input = document.querySelectorAll(".note-header__title input")[
        index
      ];

      input.classList.add("show");
    },
    closeInput(e) {
      const titleWrapper = document.querySelectorAll(".note-header__title");
      titleWrapper.forEach((item) => {
        const input = item.querySelector("input");
        if (
          input.classList.contains("show") &&
          e.target !== item.querySelector("span") &&
          e.target !== input
        ) {
          input.classList.remove("show");
        }
      });
    },
    closeInputs() {
      const inputs = document.querySelectorAll("input");
      console.log("123");
      inputs.forEach((input) => {
        if (input.classList.contains("show")) {
          input.classList.remove("show");
        }
      });
    },
  },
};
</script>

<style lang="scss"></style>
