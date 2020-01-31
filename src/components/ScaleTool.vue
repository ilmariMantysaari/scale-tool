<template>
  <div class="scale-tool">
    <Neck
      v-bind:notes="this.notes"
    />

  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator'
import Neck from './Neck.vue'

import * as R from 'ramda';

export type Note = 'A' | 'A♯' | 'B' | 'C' | 'C♯' | 'D' | 'D♯' | 'E' | 'F' | 'F♯' | 'G' | 'G♯';

export const CHROMATIC: Note[] = ['A', 'A♯', 'B', 'C', 'C♯', 'D', 'D♯', 'E', 'F', 'F♯', 'G', 'G♯']

const STANDARD_TUNING: Note[] = ['E', 'A', 'D', 'G', 'B', 'E'];

@Component({
  components: {
    Neck
  }
})
export default class ScaleTool extends Vue {
  private notes: (Note | undefined)[][] = [[]];

  created () {
    this.notes = generateNotes(STANDARD_TUNING, 15);
  }

}

const generateNotes: (
  tuning: Note[],
  fretCount: number
) => Note[][] = (tuning, fretCount) =>
  tuning.map((note: Note) => {
    const start: number = CHROMATIC.findIndex(n => n === note);
    return R.times((i) => CHROMATIC[(start + i) % CHROMATIC.length], fretCount);
  })

</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
</style>
