<template>
  <div class="container">
    <template v-for="y in board.length">
      <div
        v-for="x in board[y - 1].length"
        :key="`${y}-${x}`"
        class="cell"
        @click="onClick(x - 1, y - 1)"
      >
        <div
          v-if="hasStone(x - 1, y - 1)"
          :class="['stone', isBlack(x - 1, y - 1) ? 'black' : 'white']"
        />
      </div>
    </template>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'nuxt-property-decorator'

interface Cell {
  x: number
  y: number
  color: number
}

@Component
export default class extends Vue {
  get hasStone() {
    return (x: number, y: number): boolean => this.board[y][x] !== 0
  }

  get isBlack() {
    return (x: number, y: number): boolean => this.board[y][x] === 1
  }

  get puttableCells(): Cell[] {
    return this.board
      .flatMap((row, y) => row.map((color, x) => ({ x, y, color })))
      .filter((cell) => {
        // homework!
        return !!cell
      })
  }

  // prettier-ignore
  board = [
    [0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 1, 2, 0, 0, 0],
    [0, 0, 0, 2, 1, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0]
  ]

  currentColor = 1

  onClick(x: number, y: number) {
    const canPut = this.puttableCells.some(
      (cell) => cell.x === x && cell.y === y
    )

    if (canPut) {
      this.board = JSON.parse(JSON.stringify(this.board))
      this.board[y][x] = this.currentColor
      this.currentColor = 3 - this.currentColor
    }
  }
}
</script>

<style scoped>
.container {
  width: 480px;
  height: 480px;
  margin: 20px auto;
  background: #050;
}

.cell {
  float: left;
  width: 12.5%;
  height: 12.5%;
  border: 1px #000 solid;
}

.stone {
  width: 70%;
  height: 70%;
  margin: 15%;
  border-radius: 50%;
}

.white {
  background: #fff;
}

.black {
  background: #000;
}
</style>
