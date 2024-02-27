<template>
<div class="modalBackground" @click.self="HideModal">
    <div class="modal">
        <div class="text">
            Are you sure you want to delete {{ entity.name }} from the tracker?
        </div>
        <div class="buttons">
            <button @click="ConfirmDelete"> Delete </button>
            <button @click="HideModal"> Cancel </button>
        </div>

    </div>
</div>
</template>

<script>
export default {
    name: 'EntityDeleteModal',
    props: ['entity', 'entityType', 'index'],
    data() {
        return {

        }
    },

    methods: {

        ConfirmDelete() {
            this.$emit('ConfirmRemove')
        },
        
        HideModal() {
            this.$emit('HideModal');
        }
    }

}
</script>

<style>

</style>