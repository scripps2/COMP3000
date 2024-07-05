<template>
    <br>
    Strength: {{ entity.stats.strength }} - Bonus: {{ Math.floor(( entity.stats.strength - 10)/2) }}  <br>
    Dexterity: {{ entity.stats.dexterity }} - Bonus: {{ Math.floor(( entity.stats.dexterity - 10)/2) }}  <br> 
    Constitution: {{ entity.stats.constitution }} - Bonus: {{ Math.floor(( entity.stats.constitution - 10)/2) }} <br>
    Intelligence: {{ entity.stats.intelligence }} - Bonus: {{ Math.floor(( entity.stats.intelligence - 10)/2) }} <br> 
    Wisdom: {{ entity.stats.wisdom }} - Bonus: {{ Math.floor(( entity.stats.wisdom - 10)/2) }} <br> 
    Charisma: {{ entity.stats.charisma }} - Bonus: {{ Math.floor(( entity.stats.charisma - 10)/2) }} <br> 
    <br>
    <button v-if="!editingStats" @click="EditStats"> Edit Stats </button>
    <div class="statInputs" v-if="editingStats" > 
        Strength: <input type="number" min="1" step="1" v-model="localstats.strength"> 
        Dexterity: <input type="number" min="1" step="1" v-model="localstats.dexterity">
        Constitution: <input type="number" min="1" step="1" v-model="localstats.constitution"> 
        Intelligence: <input type="number" min="1" step="1" v-model="localstats.intelligence"> 
        Wisdom: <input type="number" min="1" step="1" v-model="localstats.wisdom"> 
        Charisma: <input type="number" min="1" step="1" v-model="localstats.charisma"> 
        <button @click="SaveStats"> Save </button> <button @click="CancelEditStats"> Cancel </button>
    </div>
</template>

<script>
export default {
    name: 'Stats',
    props: ["entity", "entityindex"],
    emits: ["EditStats"],
    data() {
        return {
            editingStats: false,
            localstats:{strength: 0, dexterity: 0, constitution: 0, intelligence: 0, wisdom: 0, charisma:0},
            localindex: null,
        }
    },
    mounted() {
        this.UpdateLocalStats();
        this.localindex = this.entityindex;
    },
    updated() {
        if(this.localindex !== this.entityindex) {
            this.CancelEditStats();
            this.localindex = this.entityindex;
        }
    },

    methods: {
        EditStats() {
            this.editingStats = true;
            this.UpdateLocalStats();
        },

        SaveStats() {
            // Emit new stats, close editing stat section
            let newstats = {...this.localstats};
            this.$emit("EditStats", newstats)
            this.editingStats = false;
        },

        CancelEditStats() {
            this.editingStats = false;
        },

        UpdateLocalStats() {
            this.localstats.strength = this.entity.stats.strength;
            this.localstats.dexterity = this.entity.stats.dexterity;
            this.localstats.constitution = this.entity.stats.constitution;
            this.localstats.intelligence = this.entity.stats.intelligence;
            this.localstats.wisdom = this.entity.stats.wisdom;
            this.localstats.charisma = this.entity.stats.charisma;
        }
    }
}
</script>

<style>
 .statInputs {
    display: grid;
    grid-template-columns: 50% 50%;
    row-gap: 1px;
    justify-content: center;
    text-align:right;
    flex-direction: column; 
    height: 100%;
    width: 25%;
    margin: auto;
 }
</style>