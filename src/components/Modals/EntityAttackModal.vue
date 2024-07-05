<template>
<div class="modalBackground" @click.self="HideModal">
    <div class="modal" >
        <b> Confirm attack</b>
        <br><br>
        <b> Attacker Info: </b>
        <div class="attackerInfo">
            <span>Attacker:</span> <span>{{ entity.name }}</span>
            <span>Attack roll:</span> <span>{{ tohitRoll }}</span>
            <span>Damage roll:</span> <span>{{ damageRoll }}</span>
            <span>Damage Type:</span> <span>{{ damageType }}</span>
        </div>
        <br>
        <b> Target Info: </b>
        <div class="targetInfo">
            <span>Target #:</span> <input type="number" step="1" v-model="attackTarget" @change="TargetChange">
            <span>Target Name:</span> <span> {{selectedTarget}} </span>
            <span>Target AC:</span> <span> {{ selectedTargetAC }} </span>
        </div>
        <br>
        <div class="buttons">
            <button @click="ConfirmAttack" :disabled="invalidTarget"> Confirm </button>
            <button @click="HideModal"> Cancel </button>
        </div>

    </div>
</div>
</template>

<script>
export default {
    name: 'EntityAttackModal',
    props: ["entity", "npcs"],
    emits: ["ConfirmAttack", "HideModal"],
    data() {
        return {
            tohitRoll: 0,
            damageRoll: 0,
            damageType: 0,
            attackTarget: null,
            invalidTarget: false,
            selectedTarget: '',
            selectedTargetAC: null,
        }
    },

    // updated() {
    //     if(Math.floor(this.attackTarget) < this.npcs.length) {
    //         this.selectedTarget = this.npcs[Math.floor(this.attackTarget)].name;
    //         this.selectedTargetAC = this.npcs[Math.floor(this.attackTarget)].armorClass;
    //     }
    //     else {
    //         this.selectedTarget = "Invalid Entry";
    //         this.selectedTargetAC = 0;
    //     }
    // },

    mounted() {
        this.tohitRoll = Math.floor(Math.random()*20)+1 + Math.floor((this.entity.stats.strength-10)/2);
        this.damageRoll = Math.max(1,1+Math.floor((this.entity.stats.strength-10)/2));
        this.damageType = "Bludgeoning";
    },

    methods: {
        TargetChange() {
            this.attackTarget = Math.floor(this.attackTarget);
            if(this.attackTarget < this.npcs.length && this.attackTarget >= 0) {
                this.selectedTarget = this.npcs[Math.floor(this.attackTarget)].name;
                this.selectedTargetAC = this.npcs[Math.floor(this.attackTarget)].armorClass;
                this.invalidTarget = false;
            } else {
                this.selectedTarget = "Invalid Entry";
                this.selectedTargetAC = 0;
                this.invalidTarget = true;
            }
        },

        ConfirmAttack() {
            if(this.selectedTargetAC <= this.tohitRoll) this.$emit('ConfirmAttack', this.attackTarget, this.damageRoll);
            else this.$emit('HideModal');
        },

        HideModal() {
            this.$emit('HideModal');
        },


    }
}
</script>

<style>

 .attackerInfo, .targetInfo {
    display: grid;
    grid-template-columns: 50% 50%;
    row-gap: 6px;
    justify-content: left;
    text-align:left;
    flex-direction: column; 
    height: 100%;
    width: 100%;
 }

 .attackerInfo span, .targetInfo span {
    border-bottom-style:solid;
    border-bottom-width:1px;
    border-color: gray;
 }

</style>