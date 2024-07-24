<template>
    <div class="notes__item" @click="dropMenuOpen = false">
      <div class="notes__item_top">
          <div class="notes__item_top_desc">
            <h3 v-html="note.title" class="notes__item_top-title"></h3>
            <p class="notes__item_top-date">{{ note.date }}</p>
          </div>
  
          <div v-if="!isScreenLarge" class="notes__item_dropmenu">
              <button @click.stop="toggleDropMenu" class="notes__item_dropmenu-btn">
                  <span></span>
                  <span></span>
                  <span></span>
              </button>
  
              <div v-if="dropMenuOpen" class="notes__item_dropmenu_btns">
                  <button @click="$emit('change', note.id)" class="notes__item_dropmenu_btns-btn edit-mobile">
                      <img src="@/assets/img/black_pen.svg" alt="pens" width="24" height="24">
                      <span>{{ $t('edit') }}</span>
                  </button>
                  <button @click="$emit('delNote', note.id)" class="notes__item_dropmenu_btns-btn del-mobile">
                      <img src="@/assets/img/del.svg" alt="basket" width="24" height="28">
                      <span>{{ $t('delete') }}</span>
                  </button>
              </div>
          </div>
      </div>
  
      <div class="notes__item_text">
          <p v-html="note.text"></p>
      </div>
  
      <div v-if="isScreenLarge" class="notes__item_btns">
        <button @click="$emit('change', note.id)" class="notes__item_btns-btn edit">
          <img src="@/assets/img/pens.svg" alt="pens">
          <span>{{ $t('edit') }}</span>
        </button>
        <button @click="$emit('delNote', note.id)" class="notes__item_btns-btn del">
          <img src="@/assets/img/basket.svg" alt="basket">
          <span>{{ $t('delete') }}</span>
        </button>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      note: {
        type: Object,
        required: true
      },
    },
    data() {
      return {
        screenWidth: window.innerWidth,
        dropMenuOpen: false,
        show: null,
      };
    },
    computed: {
      isScreenLarge() {
        this.show = this.screenWidth > 992 ? true : false
        this.$emit('show', this.show)
        return this.screenWidth > 992;
      },
    },
    mounted() {
      window.addEventListener('resize', this.updateScreenWidth);
      document.addEventListener('click', this.handleClickOutside);
    },
    beforeDestroy() {
      window.removeEventListener('resize', this.updateScreenWidth);
      document.removeEventListener('click', this.handleClickOutside);
    },
    methods: {
      updateScreenWidth() {
        this.screenWidth = window.innerWidth;
      },
      toggleDropMenu() {
        this.dropMenuOpen = !this.dropMenuOpen;
      },
      handleClickOutside(event) {
        if (this.$el && !this.$el.contains(event.target) && this.dropMenuOpen) {
          this.dropMenuOpen = false;
        }
      },
    },
  };
  </script>
  
  <style scoped>
  /* Ваши стили */
  </style>
  