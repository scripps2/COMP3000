<!-- PlayerCard consists of details relating to Player characters that will be displayed on the left.
For early prototyping, Name and Health will be displayed.
Clicking the card will change the DetailsPage to show more details about the player.
A button will bring up the ActionResolve menu to allow quickly performing an action.-->

<template>
  <div class="playercard" v-if="entity.display == true || viewHidden == true" :class="{stable: status === 'stable', dying: status === 'dying', dead: status === 'dead', hidden: entity.display === false}">
    <div class="playercardcontent" @click="playerClick">
      <div class="playername">
        <div class="name"> {{ index }}: {{ entity.name }}</div> <button @click="showoptions = !showoptions"> Options </button>
      </div>

      <div class="playerstats">
        <div class="playerhealth">
          <div v-if="status === 'alive'">
            HP: {{ entity.health }}/{{ entity.maxhp }}
          </div>
          <div v-if="status === 'dying'">
            Death Saves!
            <br>
            Success = {{ deathsavesSuccess }} <button @click="passDeathSave">Pass</button>
            <br>
            Fails = {{ deathsavesFails }} <button @click="failDeathSave">Fail</button>
          </div>
          <div v-if="status === 'stable'">
            stable
          </div>
          <div v-if="status === 'dead'">
            dead
          </div>
        </div>
        <div class="playerinitative">
          {{ entity.initative }}
        </div>
      </div> 
    </div>

    <div class="playercardbuttons" v-if="showoptions">
      <div> <button @click="move(1)">up</button><button @click="move(-1)">down</button></div>
      <button @click="playerHide"> {{ buttonText }} </button> 
      <button @click="playerFullHp"> MaxHP </button>
      <button @click="playerZeroHp"> 0HP </button>
      <button @click="playerRemove"> Delete </button>
    </div>

  </div>
</template>

<script>

export default {
    props: ['index', 'entity', 'viewHidden'],
    emits: ['entityClicked', 'moveplayercard', 'entityHide', 'entityEditHealth', 'entityRemove'],
    data() {
      return {
        deathsavesSuccess: 0,
        deathsavesFails: 0,
        status: 'alive',
        buttonText: 'Hide',
        showoptions: false,
      }
    },
    updated() {
      if(this.status === 'alive')
        if(this.entity.health <= 0) {
          this.status = 'dying';
        }
      if(this.status === 'dying' || this.status === 'stable')
        if(this.entity.health > 0) {
          this.status = 'alive';
          this.deathsavesSuccess = 0;
          this.deathsavesFails = 0;
        }
    },
    methods: {

      playerClick() {
        this.$emit('entityClicked', 'player', this.index)
      },

      move(direction) {
        this.$emit("moveplayercard",this.index,direction)
      },

      playerHide() {
        this.$emit('entityHide', 'player', this.index)
        if(this.buttonText == 'Hide') this.buttonText = 'Show';
        else this.buttonText = 'Hide'
      },

      playerFullHp() {
        this.$emit('entityEditHealth', 'player', this.index, this.entity.maxhp)
      },

      playerZeroHp() {
        this.$emit('entityEditHealth', 'player', this.index, 0)
      },

      playerRemove() {
        this.$emit('entityRemove', 'player', this.index)
      },

      passDeathSave() {
        this.deathsavesSuccess++;
        if(this.deathsavesSuccess >= 3) {
          this.status = 'stable';
          this.deathsavesSuccess = 0;
          this.deathsavesFails = 0;
        }
      },

      failDeathSave() {
        this.deathsavesFails++;
        if(this.deathsavesFails >= 3) {
          this.status = 'dead';
          this.deathsavesFails = 0;
          this.deathsavesSuccess = 0;
        }
      }
  }
}
</script>

<style>

 .playercard {
  height: 100px;
  width: 90%;
  background: limegreen;
  margin: auto;
  border-radius: 10px;
  display: flex;
  overflow: hidden;
  border-style: solid;
 }

 .playercard.stable {
  background: rgb(150,150,100);
 }

 .playercard.dying {
  background: rgb(255,100,100);
 }

 .playercard.dead {
  background: lightslategray;
 }

 .playercard.hidden {
  opacity: 60%;
 }

 .playercardcontent {
  height: 100%;
  width: 100%;
  display: block;
 }

 .playercardbuttons {
  height: 100%;
  width: 25%;
  display: grid;
  border-left-style: solid;
  justify-content: center;
  align-content: center;
 }

 .playercardbuttons button {
  font-size: 75%;
  display: block;
  width: 100%;
  text-overflow: ellipsis;
  overflow: hidden;
  
 }

 .playername {
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

 .playerstats {
  height: 75%;
  display: flex;
 }

 .playerhealth {
  width: 75%;
  margin: auto;
 }

 .playerinitative {
  border-left-style: solid;
  width: 25%;
  display: grid;
  align-items:center;
 }
 
</style>