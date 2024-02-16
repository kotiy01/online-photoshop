<template>
    <div class="drawing-space">
        <canvas class="drawing-space__canvas" ref="canvas" :width="canvasWidth" :height="canvasHeight" @mousemove="handleMouseMove" @click="handleMouseClick"></canvas>
        <div class="drawing-space__info" v-if="imageLoaded" id="coordinates">
            <div class="color-info" v-if="pipetteActive"><p>Цвет: {{ colorInfo }}</p><div class="color" :style="{ backgroundColor: colorInfo }"></div></div>
            <p>Координаты: {{ mouseCoordinates }}</p>
            <p>Исходные размеры изображения: {{ originalImageWidth }} x {{ originalImageHeight }} px</p>
        </div>
    </div>
</template>

<script>
export default {
  data() {
    return {
      canvasWidth: 800,
      canvasHeight: 600,
      imageLoaded: false,
      colorInfo: '',
      mouseCoordinates: '',
      originalImageWidth: 0,
      originalImageHeight: 0,
      clickInfoFixed: false,
      pipetteActive: false,
    };
  },
  mounted() {
    this.canvas = this.$refs.canvas;
    this.canvasContext = this.canvas.getContext('2d');
    this.canvas.addEventListener('mousemove', this.handleMouseMove);
    this.canvas.addEventListener('click', this.handleMouseClick);
  },
  methods: {
    handleImageUpload(event) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = (e) => {
          const img = new Image();
          img.src = e.target.result;
          img.onload = () => {
            this.originalImageWidth = img.naturalWidth;
            this.originalImageHeight = img.naturalHeight;
            this.$refs.canvas.getContext('2d').clearRect(0, 0, this.canvasWidth, this.canvasHeight);
            this.$refs.canvas.getContext('2d').drawImage(
              img,
              this.canvasWidth / 2 - img.width / 2,
              this.canvasHeight / 2 - img.height / 2
            );
            this.imageLoaded = true;
          };
        };
        reader.readAsDataURL(file);
      }
    },
    handleMouseMove(event) {
      if (this.imageLoaded && !this.clickInfoFixed) {
        const canvasRect = this.canvas.getBoundingClientRect();
        const mouseX = event.clientX - canvasRect.left;
        const mouseY = event.clientY - canvasRect.top;

        this.mouseCoordinates = `X: ${Math.round(mouseX) + 1}, Y: ${Math.round(mouseY) + 1}`;
        const pixelData = this.canvasContext.getImageData(Math.round(mouseX), Math.round(mouseY), 1, 1).data;
        this.colorInfo = `rgb(${pixelData[0]}, ${pixelData[1]}, ${pixelData[2]})`;
      }
    },
    handleMouseClick(event) {
      const canvasRect = this.canvas.getBoundingClientRect();
      const mouseX = event.clientX - canvasRect.left;
      const mouseY = event.clientY - canvasRect.top;

      if (this.imageLoaded) {
        this.mouseCoordinates = `X: ${Math.round(mouseX) + 1}, Y: ${Math.round(mouseY) + 1}`;
        const pixelData = this.canvasContext.getImageData(Math.round(mouseX), Math.round(mouseY), 1, 1).data;
        this.colorInfo = `rgb(${pixelData[0]}, ${pixelData[1]}, ${pixelData[2]})`;
        this.clickInfoFixed = true;
      }
    },
    togglePipette() {
      this.pipetteActive = !this.pipetteActive;
    },
  },
};
</script>

<style lang="scss">
.drawing-space {
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin-top: 40px;

    &__info {
      width: 60%;
      display: flex;
      flex-direction: row;
      justify-content: space-between;
      margin-top: 20px;
    }
}

.color {
  width: 20px;
  height: 20px;
  margin-left: 4px;
}

.color-info {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}
</style>