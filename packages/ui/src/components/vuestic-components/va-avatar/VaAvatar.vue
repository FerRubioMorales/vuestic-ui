<template>
  <div
    class="va-avatar"
    ref="avatar"
    :style="computedStyle"
  >
    <slot>
      <va-progress-circle
        v-if="loading"
        indeterminate
        :size="sizeComputed"
        :color="colorComputed"
      />
      <img
        v-else-if="src"
        :src="src"
      >
      <va-icon
        v-else-if="icon"
        :name="icon"
      />
      <img
        v-else-if="email"
        :src="computedGravarar"
      >
    </slot>
  </div>
</template>

<script lang="ts">
import { Component, Mixins } from 'vue-property-decorator'

import VaProgressCircle from '../va-progress-bar/progress-types/VaProgressCircle.vue'
import VaIcon from '../va-icon/VaIcon.vue'

// @ts-ignore
import gravatar from 'gravatar'
import { SizeMixin } from '../../../mixins/SizeMixin'
import { ColorThemeMixin, getColor } from '../../../services/ColorThemePlugin'
import { makeContextablePropsMixin } from '../../context-test/context-provide/ContextPlugin'
import { LoadingMixin } from '../../vuestic-mixins/LoadingMixin/LoadingMixin'

const AvatarPropsMixin = makeContextablePropsMixin({
  color: { type: String, default: 'info' },
  textColor: { type: String, default: 'white' },
  square: { type: Boolean, default: false },
  icon: { type: String, default: '' },
  src: { type: String, default: null },
  fontSize: { type: String, default: '' },
  email: { type: String, default: '' },
})

@Component({
  name: 'VaAvatar',
  components: { VaIcon, VaProgressCircle },
})
export default class VaAvatar extends Mixins(
  SizeMixin,
  ColorThemeMixin,
  LoadingMixin,
  AvatarPropsMixin,
) {
  get computedGravarar () {
    return gravatar.url(this.c_email, {
      s: this.sizeComputed,
      d: 'mp',
    })
  }

  get computedStyle () {
    return {
      color: getColor(this, this.c_textColor, '#ffffff'),
      backgroundColor: this.c_loading || this.c_email ? 'transparent' : this.colorComputed,
      borderRadius: this.c_square ? 0 : '50%',
      fontSize: this.c_fontSize || this.fontSizeComputed,
      width: this.sizeComputed,
      minWidth: this.sizeComputed, // We only define width because common use case would be flex row, for column we expect user to set appropriate styling externally.
      height: this.sizeComputed,
    }
  }
}
</script>

<style lang="scss">
.va-avatar {
  align-items: center;
  display: inline-flex;
  justify-content: center;
  line-height: normal;
  position: relative;
  text-align: center;
  vertical-align: middle;

  img,
  svg {
    border-radius: inherit;
    display: inline-flex;
    height: inherit;
    width: inherit;
    margin: auto;
  }
}
</style>
