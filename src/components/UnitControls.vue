<template>
    <div>
        <button v-on:click="moveUnitRequest('up')" :style="style" :disabled="disabled">⇧</button>
        <button v-on:click="moveUnitRequest('down')" :style="style" :disabled="disabled">⇩</button>
        <button v-on:click="moveUnitRequest('left')" :style="style" :disabled="disabled">⇦</button>
        <button v-on:click="moveUnitRequest('right')" :style="style" :disabled="disabled">⇨</button>
    </div>
</template>

<script>
import { mapState } from 'vuex'
import { mapGetters } from 'vuex'

export default {
    name: 'UnitControls',
    props: ['unit'],
    methods: {
        moveUnitRequest(direction) {
            this.$store.dispatch('moveUnit', { unit: this.unit, direction })
            // then once promise
            this.$store.dispatch('updateGameState')
        },
    },
    computed: {
        style() {
            return  {
                'width': '260px',
                'height': '120px',
                'margin': '3px',
                'color': this.unit.color,
                'border': '2px solid black',
                'font-size': '60px',
            }
        },
        disabled() {
            return this.game.state !== 'prize' && this.game.state !== 'exit'
        },
        ...mapState(['game']),
    },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
