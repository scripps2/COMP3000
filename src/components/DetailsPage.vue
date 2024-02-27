<!-- DetailsPage sits in the center of the page and will be used to display the information
    of the most recently clicked player or NPC. -->

<template>
    <div class="detailsname" :class="{ isplayer: entityType === 'player' }">
        {{ entity.name }}
    </div>
    <div class="detailsnavigation">
        <button @click="detailsChangeTab('main')"> Main </button> 
        <button @click="detailsChangeTab('stats')"> Stats </button>
        <button @click="detailsChangeTab('attacks')"> Attacks </button>
        <button @click="detailsChangeTab('spells')"> Spells </button>
    </div>
    <div class="detailscontent">
        <div v-if="currenttab === 'main'" class="detailsmain">
        <div class="detailshealth">
            <b> Health: {{ entity.health }}/{{ entity.maxhp }} </b>
            <div class="detailshealthinput">
                <button @click="healthChange(-1)">-</button><input type="number" step="1" v-model="healthinput" placeholder='Enter value'><button @click="healthChange(1)">+</button>
            </div>
        </div>
        <div class="detailsinitative">
            Initative: {{ entity.initative }}
        </div>
        
        </div>
        <div v-if="currenttab === 'stats'" class="stats">
        <br> 
        Strength: {{ entity.stats.strength }} - Bonus: {{ Math.floor(( entity.stats.strength - 10)/2) }}  <br>
        Dexterity: {{ entity.stats.dexterity }} - Bonus: {{ Math.floor(( entity.stats.dexterity - 10)/2) }}  <br> 
        Constitution: {{ entity.stats.constitution }} - Bonus: {{ Math.floor(( entity.stats.constitution - 10)/2) }} <br>
        Intelligence: {{ entity.stats.intelligence }} - Bonus: {{ Math.floor(( entity.stats.intelligence - 10)/2) }} <br> 
        Wisdom: {{ entity.stats.wisdom }} - Bonus: {{ Math.floor(( entity.stats.wisdom - 10)/2) }} <br> 
        Charisma: {{ entity.stats.charisma }} - Bonus: {{ Math.floor(( entity.stats.charisma - 10)/2) }} <br> 
        </div>
        <div v-if="currenttab === 'attacks'" class="attacks">
            <!-- Some sort of table system here -->
            Name of Attack - To-Hit Modifier - Damage - Extra details
            <br>
            Unarmed attack  To-hit: {{ Math.floor((entity.stats.strength-10)/2) +2 }} - Damage: {{ 1 + Math.floor((entity.stats.strength-10)/2)}}
        </div>
        <div v-if="currenttab === 'spells'" class="spells">
            <!-- Some sort of table system here -->
            Name of Spell - To-Hit or Save to roll - Damage - Extra Details
            <br>
            <!-- To hit can change between classes.  Maybe each character carries a "class" object that includes
                their class name and level as well as information like their spellcasting ability, spell slots, etc -->
            Fire Bolt - To-hit: {{ Math.floor((entity.stats.intelligence-10)/2) + 2 }} - Damage: 1d8 Fire - 120ft Range
        </div> 
    </div>
</template>

<script>
export default {
    props: ["entityIndex", "entity", "entityType"],
    emits: ["entityEditHealth"],
    data() {
        return {
            healthinput: null,
            currenttab: 'main',
        }
    },
    methods: {
        healthChange(mult) {
            var newHealth = this.entity.health + Math.floor(this.healthinput) * mult;
            this.$emit('entityEditHealth', this.entityType, this.entityIndex, newHealth)
        },

        detailsChangeTab(tabInput) {
            this.currenttab = tabInput;
        }
    }
}

</script>

<style>
    .detailsname {
        height: 5.5vh;
        font-size: 5vh;
        border-bottom-style: solid;
        line-height: 5vh;
        background: rgba(255,0,0,0.25);
        color: red;
        border-color: #2c3e50;
    }

    .detailsname.isplayer {
        background: rgba(0,255,0,0.25);
        color:darkgreen;
    }

    .detailsnavigation {
        width: 100%;
        border-bottom-style: solid;
    }

    .detailscontent {
        width: 100%;
        /*padding-top: 10px; - Want to have a margin before but have the border go all the way up */
    }
    .detailsmain {
        display: flex;
        overflow: hidden;
        border-bottom-style: solid;
    }

    .detailshealth {
        padding: 1% 0%;
        width: 75%;
        display: grid;
        align-items: center;
    }

    .detailshealthinput {
        display: flex;
        width: 100%;
        justify-content: center;
    }

    .detailsinitative {
        border-left-style: solid;
        padding: 1% 0%;
        width: 25%;
        display: grid;
        align-items:center;
    }

</style>