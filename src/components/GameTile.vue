<template>
    <div class="game-tile" :style="style"></div>
</template>

<script>
import mapConversions from '../mixins/MapConversions.js'
import { mapGetters } from 'vuex'

export default {
    name: 'GameTile',
    props: ['gameBoard', 'row', 'column', 'tile'],
    mixins: [mapConversions],
    computed: {
        style() {
            if (!this.isWallCoord(this.row, this.column)) {
                let size = this.gameBoard.tileSize
                let style = {
                    ...this.tile.style,
                    'width': `${size}px`,
                    'height': `${size}px`,
                    'left': `${this.screenColumn * size}px`,
                    'top': `${this.screenRow * size}px`,
                    'border-top': this.wallStyle('top'),
                    'border-bottom': this.wallStyle('bottom'),
                    'border-left': this.wallStyle('left'),
                    'border-right': this.wallStyle('right'),
                }
                return style
            } else {
                return { 'display': 'none' }
            }
        },
       ...mapGetters(['getNeighborWall']),
    },
    methods: {
        isWallCoord(row, column) {
            return row % 2 === 0 || column % 2 === 0
        },
        wallStyle(direction) {
            let neighbor = this.getNeighborWall(direction, this.row, this.column)
            return neighbor.type === 'wall' ? '2px solid gray' : '0px'
        },
    },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.game-tile {
    box-sizing: border-box;
    position: absolute;
}
</style>
