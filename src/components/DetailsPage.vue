<!-- DetailsPage sits in the center of the page and will be used to display the information
    of the most recently clicked player or NPC.
Deciding between having information with tabs or a scrollable area with sections.-->

<template>
    <div class="detailsname" :class="{ isplayer: player === 'player' }">
        {{ entity.name }}
    </div>
    <div class="detailsmain">
        <div class="detailshealth">
            <b> Health: {{ entity.health }} </b>
            <div class="detailshealthinput">
                <button @click="healthChange(-1)">-</button><input type="number" step="1" v-model="healthinput" placeholder='Enter value'><button @click="healthChange(1)">+</button>
            </div>
        </div>
        <div class="detailsinitative">
            Initative: {{ entity.initative }}
        </div>
        
    </div>
    <div class="stats">
        <br> 
        Strength: {{ entity.stats[0] }} - Bonus: {{ Math.floor((entity.stats[0]-10)/2) }}  <br>
        Dexterity: {{ entity.stats[1] }} - Bonus: {{ Math.floor((entity.stats[1]-10)/2) }}  <br> 
        Constitution: {{ entity.stats[2] }} - Bonus: {{ Math.floor((entity.stats[2]-10)/2) }} <br>
        Intelligence: {{ entity.stats[3] }} - Bonus: {{ Math.floor((entity.stats[3]-10)/2) }} <br> 
        Wisdom: {{ entity.stats[4] }} - Bonus: {{ Math.floor((entity.stats[4]-10)/2) }} <br> 
        Charisma: {{ entity.stats[5] }} - Bonus: {{ Math.floor((entity.stats[5]-10)/2) }} <br> 
    </div>
</template>

<script>
export default {
    props: ["entity", "player"],
    emits: ["HealthChange"],
    data() {
        return {
            healthinput: null
        }
    },
    methods: {
        healthChange(mult) {
            var newHealth = this.entity.health + Math.floor(this.healthinput) * mult;
            this.$emit('HealthChange',this.player, this.entity.id, newHealth)
            console.log(newHealth)
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