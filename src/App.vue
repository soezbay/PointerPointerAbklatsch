<template>
  <div class="grid-container">
    <div v-for="cell in gridCells" :key="cell.id" class="grid-cell" @mouseenter="showImage(cell.image)">
    </div>

    <!-- Zentrales Bild-Display -->
    <div class="image-display">
      <img v-if="currentImage" :src="currentImage" alt="pointing person" />
    </div>
  </div>
</template>

<script lang="ts">
interface GridCell {
  id: number;
  image: string;
}

export default {
  data() {
    return {
      currentImage: null as string | null,
      gridCells: [] as GridCell[]
    }
  },

  mounted() {
    this.createGrid();
  },

  methods: {
    createGrid() {
      const rows = 8;
      const cols = 8;

      for (let row = 0; row < rows; row++) {
        for (let col = 0; col < cols; col++) {
          const direction = this.calculateDirection(row, col, rows, cols);
          this.gridCells.push({
            id: row * cols + col,
            image: `/images/pointing-${direction}.jpg`
          });
        }
      }
    },

    calculateDirection(row: number, col: number, totalRows: number, totalCols: number): string {
      const centerRow = (totalRows - 1) / 2;  // 3.5
      const centerCol = (totalCols - 1) / 2;  // 3.5

      const angle = Math.atan2(row - centerRow, col - centerCol) * 180 / Math.PI;

      // 8 Richtungen basierend auf Winkeln
      if (angle >= -22.5 && angle < 22.5) return 'right';
      if (angle >= 22.5 && angle < 67.5) return 'down-right';
      if (angle >= 67.5 && angle < 112.5) return 'down';
      if (angle >= 112.5 && angle < 157.5) return 'down-left';
      if (angle >= 157.5 || angle < -157.5) return 'left';
      if (angle >= -157.5 && angle < -112.5) return 'up-left';
      if (angle >= -112.5 && angle < -67.5) return 'up';
      if (angle >= -67.5 && angle < -22.5) return 'up-right';
      return 'center';
    },

    showImage(imagePath: string) {
      this.currentImage = imagePath;
    }
  }
}
</script>

<style scoped>
.grid-container {
  position: relative;
  width: 100vw;
  height: 100vh;
  display: grid;
  grid-template-columns: repeat(8, 1fr);
  grid-template-rows: repeat(8, 1fr);
  gap: 0;
}

.grid-cell {
  background: transparent;
  cursor: none;
  border: 1px solid rgba(200, 200, 200, 0.1);
}

.image-display {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  pointer-events: none;
  z-index: 10;
}

.image-display img {
  max-width: 300px;
  height: auto;
  transition: opacity 0.15s ease;
}
</style>
