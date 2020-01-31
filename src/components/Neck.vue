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
import { Note } from './ScaleTool.vue';

interface Vector2 {
  x: number,
  y: number
}

@Component
export default class Neck extends Vue {
  @Prop() private height: number = 300;
  @Prop() private width: number = 1000;
  @Prop() private notes!: (Note | undefined)[][];

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
      drawFrets(ctx, this.width, this.height, this.notes[0].length);
      drawStrings(ctx, this.width, this.height, this.notes.length);
      drawInfo(ctx, this.width, this.height, this.notes);
    }
  }
}

const drawInfo: (
  ctx: CanvasRenderingContext2D,
  width: number,
  height: number,
  notes: (Note | undefined)[][]
) => void = (ctx, width, height, notes) => {
  const xSpacing: number = width / (notes[0].length + 1);
  const ySpacing: number = height / (notes.length + 1);

  // TODO: merkkaukset
  for(let string = 0; string < notes.length; string++) {
    for(let fret = 0; fret < notes[0].length; fret++) {
      // if ([3, 5, 7, 9, 12].includes(fret)) {
      //   ctx.fillStyle = '#dbb725';
      //   ctx.fillRect(fret * xSpacing, 0, (fret * xSpacing) + xSpacing, height);
      // }

      if (notes[string][fret]) {
        const x: number = (fret * xSpacing) + (xSpacing / 2);
        const y: number = (string + 1) * ySpacing;
        ctx.textBaseline = 'middle';
        ctx.textAlign = 'center';
        ctx.font = '20px bold';

        drawCircle(ctx, { x, y }, 12, 'white');

        ctx.fillStyle = 'black';
        // middle of fret, on top of string
        ctx.fillText(notes[string][fret] as Note, x, y);
      }
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
  ctx.lineWidth = 2;
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
  ctx.lineWidth = 3;
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
