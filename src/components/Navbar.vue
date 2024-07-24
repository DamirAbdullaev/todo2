<template>
  <header class="header">
    <nav v-if="nav" class="nav">
        <button class="nav__lang" @click="switchLang">{{ $i18n.locale }}</button>
        <h1 class="nav__title">{{ $t('notes') }}</h1>
        <button @click="nav = false" class="nav__search_icon"><img src="@/assets/img/search.svg" alt="search"></button>
    </nav>

    <nav v-else class="nav__search">
      <button @click="nav = true" class="nav__search_back">
        <img src="@/assets/img/back.svg" alt="back">
      </button>

      <input v-model="search" type="text" :placeholder="$t('search') + '...'" autofocus>

      <button class="nav__search_clear" @click="search = ''">
        <img src="@/assets/img/close.svg" alt="close">
      </button>
    </nav>
  </header>
</template>

<script>
    export default {
        data() {
          return {
            nav: true,
            search: '',
          }
        },

        methods: {
          switchLang() {
            this.$i18n.locale = this.$i18n.locale == 'ru' ? 'eng' : 'ru';
            localStorage.lang = this.$i18n.locale
        }
        },

        watch: {
          search(newVal) {
            this.$emit('setSearch', newVal)
          }
        }
    }
</script>

<style lang="scss" scoped>

</style>