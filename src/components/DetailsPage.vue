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

        <Main v-if="currenttab === 'main'"
        :entityIndex = "entityIndex"
        :entity = "entity"
        :entityType = "entityType"
        @entityEditHealth = "entityEditHealth" />

        <Stats v-if="currenttab === 'stats'"
        :entity = "entity" 
        :entityindex="entityIndex"
        @EditStats = "entityEditStats"/>

        <Attacks v-if="currenttab === 'attacks'"
        :entity = "entity" 
        @entityAttacking = "entityAttacking" />

        <Spells v-if="currenttab === 'spells'"
        :entity = "entity" />

    </div>
</template>

<script>

import Main from './DetailsPage/Main.vue'
import Stats from './DetailsPage/Stats.vue'
import Attacks from './DetailsPage/Attacks.vue'
import Spells from './DetailsPage/Spells.vue'

export default {
    name: 'DetailsPage',
    components: { Main, Stats, Attacks, Spells },
    props: ["entityIndex", "entity", "entityType"],
    emits: ["entityEditHealth", "entityAttacking", "entityEditStats"],
    data() {
        return {
            currenttab: 'main',
        }
    },
    methods: {
        entityEditHealth(newHealth) {
            this.$emit('entityEditHealth', this.entityType, this.entityIndex, newHealth)
        },

        detailsChangeTab(tabInput) {
            this.currenttab = tabInput;
        },

        entityAttacking() {
            /*this.$emit("playerAttacking", this.entityType, this.entityIndex, attackInfo);*/
            this.$emit("entityAttacking");
        },

        entityEditStats(newstats) {
            this.$emit("entityEditStats", this.entityType, this.entityIndex, newstats)
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
        border-color: black;
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
</style>