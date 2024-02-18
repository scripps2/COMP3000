<template>
<body>
  <div class="top">
    Name of Encounter
  </div>
  <div class="modalDisplay" v-if="displayModal === true">
    <EntityModal v-if="modalType === 'addEntity'"
    @HideModal="HideModal"
    @AddPlayer="AddPlayer" />

    <EntityDeleteModal v-if="modalType === 'removeEntity'" 
    :entity="entity"
    @HideModal="HideModal"
    @ConfirmRemove="ConfirmRemove"/>
  </div>
  <div class="maincontent">
    <div class="left">
      <div class="leftheader">
        Player List <br>
        <button @click="AddEntityRand('player')">Add Random</button> 
        <button @click="ShowEntityModal('player')">Add Modal</button>
        <button @click="ToggleHiddenView()">{{ hiddenText }}</button>
      </div>
      <div class="leftcontent">
        <PlayerCard 
          v-for="player in players"
          :key="player.id"
          :id="player.id"
          :name="player.name"
          :maxhp="player.maxhp"
          :health="player.health"
          :initative="player.initative"
          :display ="player.display"
          :viewHidden="this.viewHidden"
          @entityClicked="entityClicked"
          @entityHide="entityHide"
          @entityEditHealth="entityEditHealth"
          @entityRemove="entityRemove"
        />       
      </div>
    </div>
    <div class="middle">
      <div class="middleheader">
        Entity Details
      </div>
      <div class="middlecontent">
        <DetailsPage v-if="showInfo" :entity="entity" :entityType="entityType" @entityEditHealth="entityEditHealth"/>
        <div v-if="!showInfo">
          <br>
          Click a player or NPC to be able to <br>
          view and edit information about them.
        </div>
      </div>
    </div>
    <div class="right">
      <div class="rightheader">
        Enemy List <button @click="AddEntityRand('npc')">Add</button>
      </div>
      <div class="rightcontent">
        <NpcCard 
          v-for="npc in npcs"
          :key="npc.id"
          :id="npc.id"
          :name="npc.name"
          :health="npc.health"
          :initative="npc.initative"
          @entityClicked="entityClicked"
          @entityHide="entityHide"
        />    
      </div>
    </div>
  </div>
</body>
</template>

<script>
import PlayerCard from './components/PlayerCard.vue'
import DetailsPage from './components/DetailsPage.vue'
import NpcCard from './components/NpcCard.vue'
import EntityModal from './components/EntityModal.vue'
import EntityDeleteModal from './components/EntityDeleteModal.vue'

export default {
  name: 'App',
  components: { PlayerCard, DetailsPage, NpcCard, EntityModal, EntityDeleteModal },
  data() {
    return {
      players: this.GetPlayers(),
      showInfo: false,
      npcs: this.GetNPCs(),
      entity: null,
      entityType: null,
      playerTotal: null,
      npcTotal: null,
      displayModal: false,
      modalType: '',
      viewHidden: false,
      hiddenText: 'Show Hidden'
    }
  },
  mounted() { 
    // Grabs the length of players and npcs to use for assigning ids to new players/npcs
    // Allows removing characters without messing up any functions
    this.playerTotal = this.players.length;
    this.npcTotal = this.npcs.length;
  },

  methods: {
    entityClicked(targetType, targetID) {
      // Player card has been clicked, displaying their details.
      if (targetType === 'player') {
      this.entity = this.players[targetID]
      this.entityType = "player"
      this.showInfo = true
      }
      else if (targetType === 'npc') {
        this.entity = this.npcs[targetID]
        this.entityType = 'npc'
        this.showInfo = true
      }
      else console.log('Incorrect targetType given. Given targetType: ' + targetType);
    },

    GetPlayers(input) {
      // Placeholder function to generate players if none are provided.
      var playerlist;
      if(input === undefined) {
        playerlist = [
          {id: 0, name: "Player 1", display: true, maxhp: 12, health: 12, initative: 8, stats:{ strength: 15, dexterity: 13, constitution: 14, intelligence: 8, wisdom: 10, charisma: 12}},
          {id: 1, name: "2nd", display: true, maxhp: 8, health: 8, initative: 18, stats: { strength: 8, dexterity: 15, constitution: 10, intelligence: 12, wisdom: 13, charisma: 14}},
          {id: 2, name: "Third", display: true, maxhp: 6, health: 6, initative: 12, stats: { strength: 8, dexterity: 12, constitution: 10, intelligence: 15, wisdom: 14, charisma: 13 }},
        ]
      }
      else {
        playerlist = input;
      }
      return playerlist;
    },

    GetNPCs(input) {
      // Placeholder function to generate npcs if none are provided.
      var npclist;
      if(input === undefined) {
        npclist = [
          {id: 0, name: "Goblin", maxhp: 6, health: 6, initative: 4, stats: [10,10,10,10,10,10]},
          {id: 1, name: "Goblin #2", maxhp: 5, health: 5, initative: 9, stats: [10,10,10,10,10,10]}
        ]
      } else {
        npclist = input;
      }
      return npclist
    },

    AddEntityRand(targetType) {
      // Function to add a player or npc to their relative lists.
      // Will be expanded upon to instead summon a modal for the information instead of randomly generated
      var generatedHealth = Math.ceil(Math.random()*6+6); // Random value between 7 and 12
      var generatedInitative = Math.ceil(Math.random()*20); // Random value between 1 and 20
      var generatedEntity = {
        id: 0,
        name: 'placeholder',
        maxhp: generatedHealth,
        health: generatedHealth,
        initative: generatedInitative,
        stats: [10,10,10,10,10,10],
        display: true,
      }
      if (targetType === 'player') {
        generatedEntity.id = this.playerTotal;
        generatedEntity.name = 'Player #' + (this.playerTotal+1);
        this.players.push(generatedEntity);
        this.playerTotal++;
      } else if (targetType === 'npc') 
      {
        generatedEntity.id = this.npcTotal;
        generatedEntity.name = 'NPC #' + (this.npcTotal+1);
        this.npcs.push(generatedEntity);
        this.npcTotal++;
      }
      else {
        console.log('Error generating entity. Given targetType: ' + targetType);
      }
    },

    ShowEntityModal(targetType) {
      this.modalType = 'addEntity';
      this.displayModal = true;
    },

    AddPlayer(playerInfo) {
      var generatedPlayer = {
        id: this.playerTotal,
        name: playerInfo.name,
        maxhp: playerInfo.health,
        health: playerInfo.health,
        initative: playerInfo.initative,
        stats: [10,10,10,10,10,10],
        display: true
      }
        this.players.push(generatedPlayer);
        this.playerTotal++;
        this.displayModal = false;
    },

    HideModal() {
      this.displayModal = false;
      this.modalType = '';
    },

    entityHide(targetType, targetid) {
      if (targetType === 'player') this.players[targetid].display = !this.players[targetid].display;
      else if (targetType === 'npc') this.npcs[targetid].display = !this.npcs[targetid].display;
      else console.log('Incorrect targetType given. Given targetType: ' + targetType);
    },

    ToggleHiddenView() {
      this.viewHidden = !this.viewHidden;
      console.log(this.viewHidden);
      if(this.viewHidden == true) this.hiddenText = 'Hide Hidden';
        else this.hiddenText = 'Show Hidden'
    },

    entityEditHealth(targetType, targetID, newHealth) {
      // Updating an entity's health value to a new amount, automatically ensuring it doesn't go over or under limits.
      if(targetType === "player") {
        this.players[targetID].health = Math.max(Math.min(newHealth,this.players[targetID].maxhp),0);
      } else if(targetType === "npc") {
        this.npcs[targetID].health = Math.max(Math.min(newHealth,this.npcs[targetID].maxhp),0);
      }
      else {
        console.log("HealthChange did not specify player or npc");
      }
    },

    entityRemove(targetType, targetid) {

      if (targetType === 'player') { this.entity = this.players[targetid]; this.entityType = 'player'; this.displayModal = true; this.modalType = 'removeEntity'; }
      else if (targetType === 'npc') { this.entity = this.npcs[targetid]; this.entityType = 'npc'; this.displayModal = true; this.modalType = 'removeEntity'; }
      else console.log('Invalid targetType given. Given targetType: ' + targetType );
    },

    ConfirmRemove() {
      if (this.entityType === 'player') this.players.splice(this.entity.id, 1);
      else if (this.entityType === 'npc') this.npcs.splice(this.entity.id, 1);
      else console.log('ConfirmRemove unsuccessful')
      this.showInfo = false;
      this.entity = null;
      this.entityType = null;
      this.hideModal = true;
      this.modalType = '';
    }


  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  border-color: #2b3d50;
  margin-top: 0px;
}
</style>
