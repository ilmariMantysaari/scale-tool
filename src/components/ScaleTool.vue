<template>
  <div class="scale-tool">
    <Neck />

  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator'
import Neck from './Neck.vue'

export const chromatic: string[] = ['A', 'A#', 'B', 'C', 'C#', 'D', 'D#', 'E', 'F', 'F#', 'G', 'G#']

@Component({
  components: {
    Neck
  }
})
export default class ScaleTool extends Vue {

}

</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
</style>
