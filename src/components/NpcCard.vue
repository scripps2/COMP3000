<!-- NpcCard consists of details relating to NPC characters that will be displayed on the right.
For early prototyping, Name and Health will be displayed.
Clicking the card will change the DetailsPage to show more details about the NPC.
A button will bring up the ActionResolve menu to allow quickly performing an action.-->

<template>
  <div class="npccard" v-if="entity.display == true || viewHidden == true" :class="{ dead: status === 'dead', hidden: entity.display === false}">
    <div class="npccardcontent" @click="npcClick">
      <div class="npcname">
        {{ index }}: {{ entity.name }}
      </div>

      <div class="npcstats">
      <div class="npchealth">
        <div v-if="status !== 'dead'">
            Health points: {{ entity.health }}/{{ entity.maxhp }}
        </div>
        <div v-if="status === 'dead'">
            Dead
        </div>
      </div>
      <div class="npcinitative">
        {{ entity.initative }}
      </div>
      </div> 

    </div>
  
    <div class="npccardbuttons">
      <button @click="npcHide"> {{ buttonText }} </button> 
      <button @click="npcFullHp"> MaxHP </button>
      <button @click="npcZeroHp"> 0HP </button>
      <button @click="npcRemove"> Delete </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'NpcCard',
  props: ['index', 'entity', 'viewHidden', 'health'],
    emits: ['entityClicked', 'entityHide', 'entityEditHealth', 'entityRemove'],
  data() {
      return {
          status: 'alive',
          buttonText: 'Hide',
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
      this.$emit('entityClicked', 'npc', this.index)
    },

    npcHide() {
      this.$emit('entityHide', 'npc', this.index)
      if(this.buttonText == 'Hide') this.buttonText = 'Show';
      else this.buttonText = 'Hide'
    },

    npcFullHp() {
      this.$emit('entityEditHealth', 'npc', this.index, this.entity.maxhp)
    },

    npcZeroHp() {
      this.$emit('entityEditHealth', 'npc', this.index, 0)
    },

    npcRemove() {
      this.$emit('entityRemove', 'npc', this.index)
    },
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
  display: flex;
  overflow: hidden;
  border-style: solid;
 }

 .npccard.dead {
  background: lightslategrey;
 }

 .npccard.hidden {
  opacity: 60%;
 } 

 .npccardcontent {
  height: 100%;
  width: 75%;
  display: block;
 }

 .npccardbuttons {
  height: 100%;
  width: 25%;
  display: grid;
  border-left-style: solid;
  justify-content: center;
  align-content: center;
 }

 .npccardbuttons button {
  font-size: 75%;
  display: block;
  width: 100%;
  text-overflow: ellipsis;
  overflow: hidden;
  
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