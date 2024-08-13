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
    props: ["entity", "npcs", "attackDetails"],
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

    mounted() {
        this.RollToHit();
        this.RollDamage();
        this.damageType = this.attackDetails.damageType;
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

        RollToHit() {
            /* 
            mainStat and tohitBonus both add a flat amount to the roll. 
                (mainStat is the stat used for the attack, tohitBonus is used for magic weapons)
            */

            let diceroll = Math.floor(Math.random()*20)+1;
            let mainStatBonus = Math.floor((this.entity.stats[this.attackDetails.mainStat]-10)/2);
            this.tohitRoll =  diceroll + mainStatBonus + this.attackDetails.tohitBonus;
        },

        RollDamage() {
            /* 
            mainStat will add a flat amount of damage
            damageRoll will be either nothing, or a dictionary of dice rolls that need to be made.
            damageBonus is a flat amount added at the end, mostly from magical weapons.
            */

            let mainstatBonus = Math.floor((this.entity.stats[this.attackDetails.mainStat]-10)/2);
            let diceroll;
            if(this.attackDetails.damageRoll == 0) diceroll = 0;
            else {
                /*let runningtotal = 0;
                let damageRollLocal = this.attackDetails.damageRoll;
                let possibleRolls = ["d4", "d6", "d8", "d10", "d12", "d20"];
                for(const dice in possibleRolls) {
                    for(i=0;i<damageRollLocal[possibleRolls];i++) {

                    }
                }*/
                console.log("Attack requires dice rolls, to be done");
                diceroll = 0;
            }

            this.damageRoll = diceroll + mainstatBonus + this.attackDetails.damageBonus;
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