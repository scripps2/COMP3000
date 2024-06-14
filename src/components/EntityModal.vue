<template>
<div class="modalBackground" @click.self="HideModal">
    <div class="modal" >
        <b> Input the details of the {{ this.entityToAdd }} you are adding. </b>
        <div class="inputs">
            Name: <input type="string" v-model="nameInput">
            Max HP: <input type="number" step="1" v-model="healthInput">
            AC: <input type="number" step="1" v-model="acInput">
            Initative: <input type="number" step="1" v-model="initativeInput">
        </div>
        <br>
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
            acInput: null,
        }
    },

    methods: {
        AddEntity() {
            var entityData = {
                name: this.nameInput,
                health: this.healthInput,
                ac: this.acInput,
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
    row-gap: 5px;
    justify-content: left;
    text-align:left;
    flex-direction: column; 
    height: 100%;
    width: 100%;
 }

</style>