<template>
    <br>
    Strength: {{ entity.stats.strength }} - Bonus: {{ Math.floor(( entity.stats.strength - 10)/2) }}  <br>
    Dexterity: {{ entity.stats.dexterity }} - Bonus: {{ Math.floor(( entity.stats.dexterity - 10)/2) }}  <br> 
    Constitution: {{ entity.stats.constitution }} - Bonus: {{ Math.floor(( entity.stats.constitution - 10)/2) }} <br>
    Intelligence: {{ entity.stats.intelligence }} - Bonus: {{ Math.floor(( entity.stats.intelligence - 10)/2) }} <br> 
    Wisdom: {{ entity.stats.wisdom }} - Bonus: {{ Math.floor(( entity.stats.wisdom - 10)/2) }} <br> 
    Charisma: {{ entity.stats.charisma }} - Bonus: {{ Math.floor(( entity.stats.charisma - 10)/2) }} <br> 
</template>

<script>
export default {
    name: 'Stats',
    props: ["entity"]
}
</script>

<style>

</style>