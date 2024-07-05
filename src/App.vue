<template>
<body>
  <div class="top">
    Name of Encounter
  </div>
  <div class="modalDisplay" v-if="displayModal === true">
    <EntityAddModal v-if="modalType === 'addEntity'"
    :entityToAdd="entityToAdd"
    @HideModal="HideModal"
    @AddEntity="AddEntity" />

    <EntityDeleteModal v-if="modalType === 'removeEntity'" 
    :entity="selectedEntity"
    @HideModal="HideModal"
    @ConfirmRemove="ConfirmRemove" />

    <EntityAttackModal v-if="modalType === 'entityAttacking'"
    :entity="selectedEntity"
    :npcs="npcs"
    @HideModal="HideModal"
    @ConfirmAttack="ConfirmAttack" />

  </div>
  <div class="maincontent">
    <div class="left">
      <div class="leftheader" @click.self="playerMenu" :class="{showoptions: showplayeroptions}">
        Player List
        <div v-if="showplayeroptions">
        <button @click="AddEntityRand('player')">Add Random</button> 
        <button @click="ShowEntityModal('player')">Add Modal</button>
        <button @click="ToggleHiddenView('players')">{{ hiddenButton.players }}</button>
        </div>
      </div>
      <div class="leftcontent">
        <PlayerCard 
          v-for="(player,index) in sorted_players"
          :key="player.name"
          :index=index
          :entity="player"
          :health="player.health"
          :viewHidden="this.viewHidden.players"
          @entityClicked="entityClicked"
          @entityHide="entityHide"
          @entityEditHealth="entityEditHealth"
          @entityRemove="entityRemove"
          @moveplayercard="moveplayercard"
        />       
      </div>
    </div>
    <div class="middle">
      <div class="middleheader">
        Entity Details
      </div>
      <div class="middlecontent">
        <DetailsPage v-if="showInfo"
          :entityIndex="selectedEntityIndex" 
          :entity="selectedEntity" 
          :entityType="selectedEntityType" 
          @entityEditHealth="entityEditHealth"
          @entityAttacking="entityAttacking"
          @entityEditStats="entityEditStats"/>
        <div v-if="!showInfo">
          <br>
          Click a player or NPC to be able to <br>
          view and edit information about them.
        </div>
      </div>
    </div>
    <div class="right">
      <div class="rightheader" @click.self="npcMenu" :class="{showoptions: shownpcoptions}">
        Enemy List <br> 
        <button @click="AddEntityRand('npc')">Add Random</button>  
        <button @click="ShowEntityModal('npc')">Add Modal</button>  
        <button @click="ToggleHiddenView('npcs')">{{ hiddenButton.npcs }}</button>  
      </div>
      <div class="rightcontent">
        <NpcCard 
          v-for="(npc,index) in sorted_npcs"
          :key="npc.name"
          :index=index
          :entity="npc"
          :health="npc.health"
          :viewHidden="this.viewHidden.npcs"
          @entityClicked="entityClicked"
          @entityHide="entityHide"
          @entityEditHealth="entityEditHealth"
          @entityRemove="entityRemove"
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
import EntityAddModal from './components/Modals/EntityAddModal.vue'
import EntityDeleteModal from './components/Modals/EntityDeleteModal.vue'
import EntityAttackModal from './components/Modals/EntityAttackModal.vue'

export default {
  name: 'App',
  components: { PlayerCard, DetailsPage, NpcCard, EntityAddModal, EntityDeleteModal, EntityAttackModal },
  data() {
    return {
      players: this.GetPlayers(),
      showInfo: false,
      npcs: this.GetNPCs(),
      deleteIndex: null,
      /* Condense selectedEntity into a single array */
      selectedEntityIndex: null,
      selectedEntity: null,
      selectedEntityType: null,
      displayModal: false,
      modalType: '',
      viewHidden: {players: false, npcs: false},
      hiddenButton: {players: 'Show Hidden', npcs: 'Show Hidden'},
      entityToAdd: '',
      initativesorting: true,
      showplayeroptions: false,
      shownpcoptions: false,
    }
  },
  mounted() { 
    // Grabs the length of players and npcs to use for assigning ids to new players/npcs
    // Allows removing characters without messing up any functions
    this.playerTotal = this.players.length;
    this.npcTotal = this.npcs.length;
  },

  methods: {
    entityClicked(targetType, targetidx) {
      // Player card has been clicked, displaying their details.
      if (targetType === 'player') {
        this.selectedEntity = this.players[targetidx];
        this.selectedEntityType = "player";
        this.showInfo = true;
        this.selectedEntityIndex = targetidx;
      }
      else if (targetType === 'npc') {
        this.selectedEntity = this.npcs[targetidx];
        this.selectedEntityType = 'npc';
        this.showInfo = true;
        this.selectedEntityIndex = targetidx;
      }
      else console.log('Incorrect targetType given. Given targetType: ' + targetType);
    },

    GetPlayers(input) {
      // Placeholder function to generate players if none are provided.
      var playerlist;
      if(input === undefined) {
        playerlist = [
          {name: "Player 1", display: true, maxhp: 12, health: 12, initative: 8, armorClass: 13, stats:{ strength: 15, dexterity: 13, constitution: 14, intelligence: 8, wisdom: 10, charisma: 12}},
          {name: "2nd", display: true, maxhp: 8, health: 8, initative: 18, armorClass: 14, stats: { strength: 8, dexterity: 15, constitution: 10, intelligence: 12, wisdom: 13, charisma: 14}},
          {name: "Third", display: true, maxhp: 6, health: 6, initative: 12, armorClass: 11, stats: { strength: 8, dexterity: 12, constitution: 10, intelligence: 15, wisdom: 14, charisma: 13 }},
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
          {name: "Goblin", display: true, maxhp: 6, health: 6, initative: 4, armorClass: 10, stats: {strength: 10, dexterity: 10, constitution: 10, intelligence: 10, wisdom: 10, charisma:10}},
          {name: "Goblin #2", display: true, maxhp: 5, health: 5, initative: 9, armorClass: 11, stats: {strength: 10, dexterity: 10, constitution: 10, intelligence: 10, wisdom: 10, charisma:10}}
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
        name: 'placeholder',
        maxhp: generatedHealth,
        health: generatedHealth,
        armorClass: 10,
        initative: generatedInitative,
        stats: {strength: 10, dexterity: 10, constitution: 10, intelligence: 10, wisdom: 10, charisma:10},
        display: true,
      }
      if (targetType === 'player') {
        generatedEntity.name = 'Player #' + (this.players.length+1);
        this.players.push(generatedEntity);
      } 
      else if (targetType === 'npc') {
        generatedEntity.name = 'NPC #' + (this.npcs.length+1);
        this.npcs.push(generatedEntity);
      }
      else {
        console.log('Error generating entity. Given targetType: ' + targetType);
      }
    },

    ShowEntityModal(targetType) {
      this.entityToAdd = targetType;
      this.modalType = 'addEntity';
      this.displayModal = true;
    },

    AddEntity(entityType, entityInfo) {
      var generatedEntity = {
        name: entityInfo.name,
        maxhp: entityInfo.health,
        health: entityInfo.health,
        armorClass: entityInfo.ac,
        initative: entityInfo.initative,
        stats: {strength: 10, dexterity: 10, constitution: 10, intelligence: 10, wisdom: 10, charisma:10},
        display: true
      }
        if (entityType === 'player') this.players.push(generatedEntity);
        else if (entityType === 'npc') this.npcs.push(generatedEntity);
        else console.log('Error adding entity. Given type: ' + entityType);
        this.displayModal = false;
        this.entityToAdd = false;
    },

    HideModal() {
      this.displayModal = false;
      this.modalType = '';
      this.deleteIndex = null;
    },

    entityHide(targetType, targetidx) {
      if (targetType === 'player') this.players[targetidx].display = !this.players[targetidx].display;
      else if (targetType === 'npc') this.npcs[targetidx].display = !this.npcs[targetidx].display;
      else console.log('Incorrect targetType given. Given targetType: ' + targetType);
    },

    ToggleHiddenView(entityType) {
      if (entityType === 'players') { 
        this.viewHidden.players = !this.viewHidden.players; 
        if(this.viewHidden.players == true) this.hiddenButton.players = 'Hide Hidden';
        else this.hiddenButton.players = 'Show Hidden' }

      else if (entityType === 'npcs') {
        this.viewHidden.npcs = !this.viewHidden.npcs;
        if(this.viewHidden.npcs == true) this.hiddenButton.npcs = 'Hide Hidden';
        else this.hiddenButton.npcs = 'Show Hidden' }
    },

    moveplayercard(targetidx, direction) {
      if (this.players[targetidx-direction]) { //Check the selected player card isn't the first or last one
        this.initativesorting = false;
        var temp = this.players[targetidx];
        this.players[targetidx] = this.players[targetidx-direction];
        this.players[targetidx-direction] = temp;
      }
    },

    entityEditHealth(targetType, targetidx, newHealth) {
      // Updating an entity's health value to a new amount, automatically ensuring it doesn't go over or under limits.
      if(targetType === "player") {
        this.players[targetidx].health = Math.max(Math.min(newHealth,this.players[targetidx].maxhp),0);
      } else if(targetType === "npc") {
        this.npcs[targetidx].health = Math.max(Math.min(newHealth,this.npcs[targetidx].maxhp),0);
      }
      else {
        console.log("HealthChange did not specify player or npc");
      }
    },

    entityAttacking() {
      /* Trigger a modal on who the target will be */
      this.displayModal = true;
      this.modalType = "entityAttacking";
    },

    ConfirmAttack(attackTarget, damageRoll) {
      Math.max(Math.min(this.npcs[attackTarget].health -= damageRoll,this.npcs[attackTarget].maxhp),0);
      this.displayModal = false;
      this.modalType = '';
    },

    entityEditStats(entityType, entityIndex, newstats) {
      if(entityType === "player") this.players[entityIndex].stats = newstats;
      else if(entityType === "npc") this.npcs[entityIndex].stats = newstats;
    },

    entityRemove(targetType, targetidx) {
      this.deleteIndex = targetidx;
      if (targetType === 'player') { this.selectedEntity = this.players[targetidx]; this.entityType = 'player'; this.displayModal = true; this.modalType = 'removeEntity'; }
      else if (targetType === 'npc') { this.selectedEntity = this.npcs[targetidx]; this.entityType = 'npc'; this.displayModal = true; this.modalType = 'removeEntity'; }
      else console.log('Invalid targetType given. Given targetType: ' + targetType );
    },

    ConfirmRemove() {
      if (this.entityType === 'player') this.players.splice(this.deleteIndex, 1);
      else if (this.entityType === 'npc') this.npcs.splice(this.deleteIndex, 1);
      else console.log('ConfirmRemove unsuccessful')
      this.deleteIndex = null;
      this.showInfo = false;
      this.selectedEntity = null;
      this.selectedEntityType = null;
      this.hideModal = true;
      this.modalType = '';
    },

    playerMenu() {
      this.showplayeroptions = !this.showplayeroptions
    },

    npcMenu() {
      this.shownpcoptions = !this.shownpcoptions
    }

  },

  computed: {
    sorted_players() {
      if (this.initativesorting === true) return this.players.sort((a,b) => {return b.initative - a.initative;});
      else return this.players;
    },

    sorted_npcs() {
      return this.npcs.sort((a,b) => {return b.initative - a.initative;});
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: black;
  border-color: black;
  margin-top: 0px;
}

.leftheader.showoptions, .rightheader.showoptions{
  height: 10%
}

</style>
