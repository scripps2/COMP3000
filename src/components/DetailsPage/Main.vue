<template>
<div class="detailsmain">
    <div class="detailshealth">
        <b> Health: {{ entity.health }}/{{ entity.maxhp }} </b>
        <div class="detailshealthinput">
            <button @click="healthChange(-1)">-</button><input type="number" step="1" v-model="healthinput" placeholder='Enter value'><button @click="healthChange(1)">+</button>
        </div>
    </div>
    <div class="detailsinitative">
        Initative:<br>{{ entity.initative }}
    </div>
    <div class="detailsArmorClass">
        Armor Class:<br>{{ entity.armorClass }}
    </div>
</div>
</template>

<script>
export default {
    name: 'Main',
    props: ["entityIndex", "entity", "entityType"],
    emits: ["entityEditHealth"],

    data() {
        return { 
            healthinput: null,
        }
    },
    methods: {
        healthChange(mult) {
            var newHealth = this.entity.health + Math.floor(this.healthinput) * mult;
            this.$emit('entityEditHealth', newHealth)
        },
    }
}
</script>

<style>
    .detailsmain {
        display: flex;
        overflow: hidden;
        border-bottom-style: solid;
    }

    .detailshealth {
        padding: 1% 0%;
        width: 75%;
        display: grid;
        align-items: center;
    }

    .detailshealthinput {
        display: flex;
        width: 100%;
        justify-content: center;
    }

    .detailsinitative {
        border-left-style: solid;
        padding: 1% 0%;
        width: 25%;
        display: grid;
        align-items:center;
    }

    .detailsArmorClass {
        border-left-style: solid;
        padding: 1% 0%;
        width: 25%;
        display: grid;
        align-items:center;
    }
</style>