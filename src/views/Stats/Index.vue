<template>
  <div class="stats">
    <HeaderTitle title="Contributors stats 🚀" />
    <section class="row" v-if="loaded">
      <StatsWidget class="mr-2" title="Contributors" :value="contributorsCount" />
      <ColorWidget class="ml-2" title="Average Color" :value="averageColor" />
      <ColorGridWidget class="mt-4" :colors="colors" />
    </section>
    <Loader v-else />
  </div>
</template>

<script>

import HeaderTitle from '@/components/HeaderTitle'
import Loader from '@/components/Loader'
import contributors from '@/assets/contributors.json'
import StatsWidget from './StatsWidget'
import ColorWidget from './ColorWidget'
import ColorGridWidget from './ColorGridWidget'

import { colorHelper } from '@/modules/color'

export default {
  name: 'Stats',
  components: { HeaderTitle, ColorWidget, ColorGridWidget, StatsWidget, Loader },
  data: () => ({
    contributors,
    colorHelper,
    loaded: false,
    averageColor: undefined,
    contributorsCount: contributors.length.toString()

  }),
  async mounted () {
    this.averageColor = await this.getRGBAverage()
    this.loaded = true
  },
  methods: {
    async getRGBAverage () {
      const colors = this.colors.map(c => c.replace(/#/g, ''))
      const rgbValue = await this.colorHelper.getRGBAverageFromHex(colors)
      const jsonColor = await this.colorHelper.getJsonColor(rgbValue, 'rgb')

      return jsonColor.hex.value
    }
  },
  computed: {
    colors () {
      return this.contributors.map(c => c.color)
    }
  }
}
</script>

<style lang="stylus">
.stats-dashboard
  display: flex
  justify-content: space-evenly
</style>
