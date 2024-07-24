<template>
    <Transition name="modal">
        <div class="modal" @click="closeModal">
            <div v-if="show" class="modal__block" @click.stop>
                <h2 class="modal__block_title">{{ edit ? $t('editNote') : $t('addNote') }}</h2>

                <div class="modal__block_inputs">
                    <label>
                        <span>{{ $t('title') }}</span>
                        <input v-model="title" :placeholder="$t('title')" type="text">
                    </label>
                    <label>
                        <span>{{ $t('content') }}</span>
                        <textarea v-model="text" :placeholder="$t('content')" name="" id="" cols="30" rows="10"></textarea>
                    </label>
                </div>

                <div class="modal__block_btns">
                    <button @click="closeModal" class="modal__block_btns-btn del">{{ $t('cancel') }}</button>
                    <button v-if="edit" @click="changeNote" class="modal__block_btns-btn edit">{{ $t('change') }}</button>
                    <button v-else @click="addNote" class="modal__block_btns-btn edit">{{ $t('add') }}</button>
                </div>
            </div>

            <div v-else class="modal__block_mobile" @click.stop>
                <div class="modal__block_mobile_nav">
                    <button @click="closeModal" class="modal__block_mobile_nav_back">
                        <img src="@/assets/img/back.svg" alt="back">
                    </button>

                    <h2 class="modal__block__mobile_nav-title">{{ edit ? $t('editNote') : $t('addNote') }}</h2>

                    <div></div>
                </div>

                <div class="modal__block_mobile_inputs">
                    <label>
                        <span>{{ $t('title') }}</span>
                        <input v-model="title" :placeholder="$t('title')" type="text">
                    </label>
                    <label>
                        <span>{{ $t('content') }}</span>
                        <textarea v-model="text" :placeholder="$t('content')" name="" id="" cols="30" rows="10"></textarea>
                    </label>
                </div>

                <div class="modal__block_mobile_btns">
                    <button v-if="edit" @click="changeNote" class="modal__block_mobile_btns-btn edit">{{ $t('change') }}</button>
                    <button v-else @click="addNote" class="modal__block_mobile_btns-btn edit">{{ $t('add') }}</button>
                </div>
            </div>
        </div>
    </Transition>
</template>

<script>
import { v4 as uuidv4 } from 'uuid';

export default {
    props: {
        edit: {
            typeof: Boolean
        },

        editedNote: {
            typeof: Object
        }
    },
    data() {
        return {
            title: '',
            text: '',
            show: false,
        }
    },

    mounted() {
      window.addEventListener('resize', this.updateScreenWidth);
    },
    beforeDestroy() {
      window.removeEventListener('resize', this.updateScreenWidth);
    },

    computed: {
      isScreenLarge() {
        return this.screenWidth > 992;
      },
    },

    methods: {
        closeModal() {
            this.$emit('close')
            this.title = this.text = ''
        },

        updateScreenWidth() {
            this.show = this.screenWidth > 992 ? true : false
        this.screenWidth = window.innerWidth;
        },

        addNote() {
            if (this.title && this.text) {
                const note = {id: uuidv4(), title: this.title, text: this.text, date: new Date().toLocaleDateString() };
                this.$emit('addNote', note);
                this.title = this.text = ''
            }
        },

        changeNote() {
            if (this.title || this.text) {
                let newEditedNote = {id: this.editedNote.id, title: this.title, text: this.text, date: new Date().toLocaleDateString() };
                this.$emit('changedNote', newEditedNote);

                    this.title = this.text = ''

            }
        }
    },
}
</script>
