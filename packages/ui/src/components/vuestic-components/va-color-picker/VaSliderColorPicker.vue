<template>
  <SliderPicker
    v-model="valueProxy"
    class="vuestic-slider-picker"
  />
</template>

<script lang="ts">
import { Component, Vue, Prop } from 'vue-property-decorator'
// @ts-ignore
import { Slider } from 'vue-color'

@Component({
  name: 'VaColorPickerInput',
  components: {
    SliderPicker: Slider,
  },
})
export default class VaColorPickerInput extends Vue {
  @Prop({
    type: String,
    default: '',
  }) readonly value!: string

  get valueProxy (): any {
    return this.value
  }

  set valueProxy (value: any) {
    this.$emit('input', value.hex)
  }
}
</script>

<style>
.vuestic-slider-picker {
  max-width: 100%;
  padding: 8px;
}
</style>
