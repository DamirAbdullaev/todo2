<template>
    <div class="notes container">
        <div v-if="show" class="notes__top">
            <h2 class="notes__top_title">{{ notes.length > 0 ? $t('allNotes') : $t('noNotes') }}</h2>
            <button @click="view = !view" class="notes__top_btn">
                <img v-show="view" src="@/assets/img/layout.svg" alt="lsit">
                <img v-show="!view" src="@/assets/img/list.svg" alt="layout">
                <span>{{ view ? $t('grid') : $t('list') }}</span>
            </button>
        </div>

        <div class="notes__list" :class="{ active: !view }">
            <NotesItem @change="$emit('change', note.id)" @delNote="$emit('delNote', note.id)" v-for="note in notes" :key="note.id" :note="note" @show="show = $event"/>
        </div> 
    </div>
</template>

<script>
import NotesItem from './NotesItem.vue'
export default {
    components: { NotesItem },

    data() {
        return {
            view: true,
            show: null,
        }
    },

    props: {
        notes: {
            typeof: Array
        },
    },

    methods: {
    },
}
</script>