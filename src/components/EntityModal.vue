<template>
<div class="modalBackground" @click.self="HideModal">
    <div class="modal" >
        <b> Input the details of the {{ this.entityToAdd }} you are adding. </b>
        <div class="inputs">
            Name: <input type="string" v-model="nameInput">
            Health: <input type="number" step="1" v-model="healthInput">
            Initative: <input type="number" step="1" v-model="initativeInput">
        </div>
        <div class="buttons">
            <button @click="AddEntity"> Add </button>
            <button @click="HideModal"> Cancel </button>
        </div>

    </div>
</div>
</template>

<script>
export default {
    name: 'EntityModal',
    props: ["entityToAdd"],
    data() {
        return {
            nameInput: null,
            healthInput: null,
            initativeInput: null,
        }
    },

    methods: {

        AddEntity() {
            var entityData = {
                name: this.nameInput,
                health: this.healthInput,
                initative: this.initativeInput,
            }
            this.$emit('AddEntity', this.entityToAdd, entityData);
        },

        HideModal() {
            this.$emit('HideModal');
        },


    }
}
</script>

<style>

 .inputs {
    display: grid;
    grid-template-columns: 25% 75%;
    justify-content: left;
    flex-direction: column; 
    height: 100%;
    width: 100%;
 }

</style>