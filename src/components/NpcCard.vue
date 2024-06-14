<!-- NpcCard consists of details relating to NPC characters that will be displayed on the right.
For early prototyping, Name and Health will be displayed.
Clicking the card will change the DetailsPage to show more details about the NPC.
A button will bring up the ActionResolve menu to allow quickly performing an action.-->

<template>
  <div class="npccard" v-if="entity.display == true || viewHidden == true" :class="{ dead: status === 'dead', hidden: entity.display === false}">
    <div class="npccardcontent" @click="npcClick">
      <div class="npcname">
        <div class="name"> {{ index }}: {{ entity.name }}</div> <button @click="showoptions = !showoptions"> Options </button>
      </div>

      <div class="npcdetails">
          <div v-if="status === 'alive'" class="npchealth">
            <div class="npchealthtext"> Health: </div>
            <div class="healthBarBackground">
              <div class="healthBarProgress" :style="{width: this.healthPercent+'%'}"> </div>
              <span>{{ entity.health }}/{{ entity.maxhp }}</span>
            </div>
          </div>
          <div v-if="status === 'dead'" class="npchealth">
            dead
          </div>
        <div class="npcstats">
          <div class="npcinitative"> Initiative: {{ entity.initative }} </div> <div class="npcac"> AC: {{ entity.armorClass }} </div>
        </div> 
      </div>
    </div>
  
    <div class="npccardbuttons" v-if="showoptions">
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
          showoptions: false
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

  computed: {
    healthPercent() {
      return Math.floor(100*this.entity.health/this.entity.maxhp);
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
  background: rgb(255, 90, 90);
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
  width: 100%;
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

 .name {
  width:100%;
 }

 .npcdetails {
  height: 75%;
  width: 100%;
  display: block;
 }
 
 .npchealth {
  width: 100%;
  height: 50%;
  justify-content: center;
  align-items: center;
  display:flex;
  border-bottom-style:solid;
 }

 .npchealthtext {
  text-align: center;
  margin:auto;
 }

 .healthBarBackground {
  width: 75%;
  height: 20px;
  background: red;
  position:relative;
  margin:auto;
  border-radius:20px;
  overflow:hidden;
  border-style:solid;
  border-color:black;
 }

 .healthBarBackground span{
  position: absolute;
  left:0;
  right:0;
  top:0;
  bottom:0;
  margin:auto;
  color:white;
  align-content:center;
  text-shadow:1pt;
 }

 .healthBarProgress {
  height: 100%;
  width: 50%;
  background: green;
  position: relative;
 }


 .npcstats {
  width: 100%;
  height: 50%;
  align-items:center;
  display:flex;
 }

 .npcac {
  width: 50%;
  height: 100%;
  border-left-style: solid;
  text-align: center;
  align-content: center;
 }

 .npcinitative {
  width: 50%;
  height: 100%;
  text-align: center;
  align-content: center;
 }
</style>