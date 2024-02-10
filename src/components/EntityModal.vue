<template>
<div class="background">
    <div class="modal">
        <div class="inputs">
            Name: <input v-model="nameInput">
            Health: <input type="number" step="1" v-model="healthInput">
            Initative: <input type="number" step="1" v-model="initativeInput">
        </div>
        <div class="buttons">
            <button @click="AddPlayer"> Add </button>
            <button @click="HideModal"> Hide </button>
        </div>

    </div>
</div>
</template>

<script>
export default {
    name: 'EntityModal',
    data() {
        return {
            nameInput: null,
            healthInput: null,
            initativeInput: null,
        }
    },

    methods: {

        AddPlayer() {
            var playerData = {
                name: this.nameInput,
                health: this.healthInput,
                initative: this.initativeInput,
            }
            this.$emit('AddPlayer', playerData);
        },

        HideModal() {
            this.$emit('HideModal');
        },


    }
}
</script>

<style>
 .background {
    top: 0;
    position: fixed;
    background: rgba(0,0,0,0.5);
    height: 100%;
    width: 100%;
 }

 .modal {
    padding: 50px;
    width: 20%;
    margin: 100px auto;
    background: white;
    border-radius: 10px;
    /* display: flex; */
 }

 .inputs {
    display: grid;
    grid-template-columns: 25% 75%;
    justify-content: left;
    flex-direction: column; 
    height: 100%;
    width: 100%;
 }
 
 .row {
    width: 100%;
    display: flex;
    justify-content: left;
 }


</style>