<template>
  <div class="neck">
    moro
    <canvas
      ref="neckCanvas"
      id="neck-canvas"
      :width="width"
      :height="height"
    ></canvas>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator'

interface Vector2 {
  x: number,
  y: number
}

@Component
export default class Neck extends Vue {
  @Prop() private height: number = 150;
  @Prop() private width: number = 600;
  @Prop() private notes!: string[][];

  private canvas!: HTMLCanvasElement;
  $refs!: {
    neckCanvas: HTMLCanvasElement
  }

  created () {
    console.log('Moro :---DDDD');
  }
  mounted () {
    this.canvas = this.$refs.neckCanvas;
    const ctx = this.canvas.getContext('2d');
    if(ctx) {
      ctx.fillStyle = '#FFD633';
      ctx.fillRect(0, 0, this.width, this.height);
      drawFrets(ctx, this.width, this.height, 22);
      drawStrings(ctx, this.width, this.height, 6);
      drawInfo(ctx, this.width, this.height, 6, 22, this.notes);
    }
  }
}

const drawInfo: (
  ctx: CanvasRenderingContext2D,
  width: number,
  height: number,
  stringCount: number,
  fretCount: number,
  info: string[][]
) => void = (ctx, width, height, stringCount, fretCount, info) => {
  const xSpacing: number = width / (fretCount + 1);
  const ySpacing: number = height / (stringCount + 1);

  for(let i = 0; i <= stringCount; i++) {
    for(let j = 0; j <= fretCount; j++) {
      const x: number = (j * xSpacing) + (xSpacing / 2);
      const y: number = (i + 1) * ySpacing;
      ctx.textBaseline = 'middle';
      ctx.textAlign = 'center';

      drawCircle(ctx, { x, y }, 5, 'white');

      ctx.fillStyle = 'black';
      // middle of fret, on top of string
      ctx.fillText(info[i][j], x, y);
    }
  }
}

const drawStrings: (
  ctx: CanvasRenderingContext2D,
  width: number,
  height: number,
  count: number
) => void = (ctx, width, height, count) => {
  const spacing: number = height / (count + 1);
  ctx.lineWidth = 3;
  for(let i = 1; i <= count; i++) {
    drawLine(ctx, { x: 0, y: i * spacing }, { x: width, y: i * spacing })
  }
}

const drawFrets: (
  ctx: CanvasRenderingContext2D,
  width: number,
  height: number,
  count: number
) => void = (ctx, width, height, count) => {
  // one fret padding in the end
  const spacing: number = width / (count + 1);
  ctx.lineWidth = 1;
  for(let i = 1; i <= count; i++) {
    drawLine(ctx, { x: i * spacing, y: 0 }, { x: i * spacing, y: height })
  }
}

const drawCircle: (
  ctx: CanvasRenderingContext2D,
  pos: Vector2,
  rad: number,
  color: string
) => void = (ctx, pos, rad, color) => {
  ctx.beginPath();
  ctx.arc(pos.x, pos.y, rad, 0, 2 * Math.PI, false);
  ctx.fillStyle = color;
  ctx.fill();
  ctx.lineWidth = 1;
  ctx.strokeStyle = color;
  ctx.stroke();
}

const drawLine: (
  ctx: CanvasRenderingContext2D,
  start: Vector2,
  end: Vector2
) => void = (ctx, start, end) => {
  ctx.beginPath();
  ctx.moveTo(start.x, start.y);
  ctx.lineTo(end.x, end.y);
  ctx.stroke();
  ctx.closePath();
}

</script>

<style scoped>

</style>
