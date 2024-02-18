<!-- PlayerCard consists of details relating to Player characters that will be displayed on the left.
For early prototyping, Name and Health will be displayed.
Clicking the card will change the DetailsPage to show more details about the player.
A button will bring up the ActionResolve menu to allow quickly performing an action.-->

<template>
  <div v-if="display == true || viewHidden == true" class="playercard" :class="{stable: status === 'stable', dying: status === 'dying', dead: status === 'dead', hidden: display === false}">
    <div class="playercardcontent" @click="playerClick">
      <div class="playername">
        {{ name }}
      </div>

      <div class="playerstats">
        <div class="playerhealth">
          <div v-if="status === 'alive'">
            HP: {{ health }}/{{ maxhp }}
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
          {{ initative }}
        </div>
      </div> 
    </div>

    <div class="playercardbuttons">
      <button @click="playerHide"> {{ buttonText }} </button> 
      <button @click="playerFullHp"> MaxHP </button>
      <button @click="playerZeroHp"> 0HP </button>
      <button @click="playerRemove"> Delete </button>
    </div>

  </div>
</template>

<script>

export default {
    props: ['id', 'name', 'maxhp', 'health','initative', 'display', 'viewHidden', 'stats'],
    emits: ['entityClicked', 'entityHide', 'entityEditHealth', 'entityRemove'],
    data() {
      return {
        deathsavesSuccess: 0,
        deathsavesFails: 0,
        status: 'alive',
        buttonText: 'Hide',
      }
    },
    updated() {
      if(this.status === 'alive')
        if(this.health <= 0) {
          this.status = 'dying';
        }
      if(this.status === 'dying' || this.status === 'stable')
        if(this.health > 0) {
          this.status = 'alive';
        }
    },
    methods: {

      playerClick() {
        this.$emit('entityClicked', 'player', this.id)
      },

      playerHide() {
        this.$emit('entityHide', 'player', this.id)
        if(this.buttonText == 'Hide') this.buttonText = 'Show';
        else this.buttonText = 'Hide'
      },

      playerFullHp() {
        this.$emit('entityEditHealth', 'player', this.id, this.maxhp)
      },

      playerZeroHp() {
        this.$emit('entityEditHealth', 'player', this.id, 0)
      },

      playerRemove() {
        this.$emit('entityRemove', 'player', this.id)
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
  width: 75%;
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