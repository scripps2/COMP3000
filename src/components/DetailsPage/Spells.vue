<template>
    <br>
    <!-- Some sort of table system here -->
    Name of Spell - To-Hit or Save to roll - Damage - Extra Details
    <br>
    <!-- To hit can change between classes.  Maybe each character carries a "class" object that includes
        their class name and level as well as information like their spellcasting ability, spell slots, etc -->
    Fire Bolt - To-hit: {{ Math.floor((entity.stats.intelligence-10)/2) + 2 }} - Damage: 1d8 Fire - 120ft Range
</template>

<script>
export default {
    name: 'Spells',
    props: ["entity"]
}
</script>

<style>

</style>