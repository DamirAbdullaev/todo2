<template>
  <div>
    <Navbar @setSearch="searchValue = $event"/>
    <Notes :notes="filterNotes" @delNote="delNote" @change="change"/>
    <Modal v-show="isModalOpen" @close="closeWithDelay" @addNote="addNote" :edit="edit" :editedNote="editedNote" @changedNote="changedNote"/>

    <button @click="isModalOpen = true" class="add__note" v-show="!isModalOpen">
      <img src="@/assets/img/pens.svg" alt="pens">
    </button>
  </div>
</template>

<script>
import Modal from './components/Modal.vue'
import Navbar from './components/Navbar.vue'
import Notes from './components/Notes.vue'

export default {
  components: { Navbar, Notes, Modal },
  
  data() {
    return {
        notes:[],
        isModalOpen: false,
        edit: false,
        editedNote: null,
        searchValue: '',
    }
  },

  methods: {
    addNote(note) {
      this.notes.unshift(note)
      this.isModalOpen = false
    },

    getNotes() {
      let localNotes = localStorage.notes
      if (localNotes) {
        this.notes = JSON.parse(localNotes)
      }
    },

    closeWithDelay() {
      this.isModalOpen = false;
      setTimeout(() => {
        this.edit = false;
      }, 400);
    },

    delNote(id) {
      let index = this.notes.findIndex(note => note.id == id)

      this.notes.splice(index, 1)
    },

    change(id) {
      this.isModalOpen = this.edit = true
      let currentNote = this.notes.find(note => note.id == id)
      this.editedNote = currentNote
    },

    changedNote(newNote) {
      this.notes.forEach(note => {
        if (newNote.id == note.id) {
          if (newNote.title.length > 0) {
            note.title = newNote.title
            note.date = newNote.date
            this.closeWithDelay()
          }
          
          if (newNote.text.length > 0) {
            note.text = newNote.text
            this.closeWithDelay()
          }
        }
      })
    }
  },

  computed: {
    filterNotes() {
      return this.searchValue ? this.notes.filter(note => note.title.toLowerCase().includes(this.searchValue.toLocaleLowerCase())) : this.notes
    }
  },

  watch: {
    notes: {
      handler() {
        localStorage.notes = JSON.stringify(this.notes)
      },
      deep: true
    }
  },

  mounted() {
    this.getNotes()
  },
}
</script>

<style>

</style>
