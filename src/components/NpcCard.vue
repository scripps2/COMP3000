<!-- NpcCard consists of details relating to NPC characters that will be displayed on the right.
For early prototyping, Name and Health will be displayed.
Clicking the card will change the DetailsPage to show more details about the NPC.
A button will bring up the ActionResolve menu to allow quickly performing an action.-->

<template>
  <div class="npccard" @click="npcClick" :class="{ dead: status === 'dead'}">
    <div class="npcname">
      {{ name }}
    </div>

    <div class="npcstats">
      <div class="npchealth">
        <div v-if="status !== 'dead'">
            Health points: {{ health }}
        </div>
        <div v-if="status === 'dead'">
            Dead
        </div>
      </div>
      <div class="npcinitative">
        {{ initative }}
      </div>
    </div> 

  </div>
</template>

<script>
export default {
    props: ['id', 'name', 'health','initative'],
    data() {
        return {
            status: 'alive'
        }
    },
    updated() {
        if(this.health <= 0) {
            this.status = 'dead'
        }
        if(this.health > 0) {
            this.status = 'alive'
        }
    },
    methods: {
      npcClick() {
        this.$emit('detailsNpc', this.id)
    }
  }
}
</script>

<style>
 .npccard {
  width: 90%;
  height: 100px;
  background: red;
  margin: auto;
  border-radius: 10px;
  display: block;
  overflow: hidden;
  border-style: solid;
 }

 .npccard.dead {
    background: lightslategrey;
 }

 .npcname {
  height:25%;
  display: flex;
  align-items: center;
  justify-content: center;
  border-bottom-style: solid;
  font-size: 100%;
 }

 .npcstats {
  height: 75%;
  display: flex;
 }
 .npchealth {
  width: 75%;
  margin: auto;
 }

 .npcinitative {
  border-left-style: solid;
  width: 25%;
  display: grid;
  align-items:center;
 }
</style>