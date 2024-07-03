<template>
    <!-- Some sort of table system here -->
    Name of Attack - To-Hit Modifier - Damage - Extra details
    <br>
    Unarmed attack  To-hit: {{ Math.floor((entity.stats.strength-10)/2) +2 }} - Damage: {{ 1 + Math.floor((entity.stats.strength-10)/2)}}
</template>

<script>
export default {
    name: 'Attacks',
    props: ["entity"]
}
</script>

<style>

</style>