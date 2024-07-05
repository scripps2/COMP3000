<template>
    <br>
    <!-- Some sort of table system here -->
    Name of Attack - To-Hit Modifier - Damage - Extra details
    <br>
    <button @click="clickAttack()">Unarmed attack</button>  To-hit: {{ Math.floor((entity.stats.strength-10)/2) +2 }} - Damage: {{ Math.max(1,1 + Math.floor((entity.stats.strength-10)/2))}}
</template>

<script>
export default {
    name: 'Attacks',
    props: ["entity"],
    emits: ["entityAttacking"],
    methods: {
        clickAttack() {

            /*
            tohitStat is the stat used for the attack 
            - (For simplicity in implementation, only strength)

            damageType is the type of damage.
            - Bludgeoning, Slashing, Fire, etc.
            - For simplicity, Bludgeoning is implemented.

            damageRoll is the damage of the weapon
            - 2d4+2 means the result of a two random numbers from 1-4 added together, plus an additional 2.
            - For now, damage is 1 + strength, the default for an unarmed attack
            */
            
            /*let diceroll;
            let attackInfo = { tohitRoll: 0, damageType: selectedDamageType, damageRoll: 0};

            /* tohit 
            diceroll = Math.floor((Math.random()*20)+1);
            attackInfo.tohitRoll = diceroll + (Math.floor((this.entity.stats.strength-10)/2)+2);

            /* damage 
            attackInfo.damageRoll = Math.min(1, 1+(Math.floor((this.entity.stats.strength-10)/2)));

            this.$emit('playerAttacking', attackInfo)*/

            this.$emit('entityAttacking');
        }
    }
}
</script>

<style>

</style>