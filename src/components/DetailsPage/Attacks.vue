<template>
    <br>
    <!-- Some sort of table system here -->
    Name of Attack - To-Hit Modifier - Damage - Extra details
    <br>
    <button @click="clickAttack('strength', 2, 0, 1, 'bludgeoning')">Unarmed attack</button>  To-hit: {{ Math.floor((entity.stats.strength-10)/2) +2 }} - Damage: {{ Math.max(1,1 + Math.floor((entity.stats.strength-10)/2))}}
</template>

<script>
export default {
    name: 'Attacks',
    props: ["entity"],
    emits: ["entityAttacking"],
    methods: {
        clickAttack(mainStat, tohitBonus, damageRoll, damageBonus, damageType) {

            let attackInfo = {mainStat: mainStat, tohitBonus: tohitBonus, damageRoll: damageRoll, damageBonus: damageBonus, damageType: damageType};
            
            this.$emit('entityAttacking', attackInfo)
        }
    }
}
</script>

<style>

</style>