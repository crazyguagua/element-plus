<template>
  <div
    class="el-tabs__active-bar"
    :class="`is-${ rootTabs.props.tabPosition }`"
    :style="barStyle"
  ></div>
</template>
<script lang='ts'>
import { defineComponent, inject, getCurrentInstance, watch, nextTick, ref } from 'vue'
import { firstUpperCase } from '@element-plus/utils/util'

export default defineComponent({
  name: 'ElTabBar',
  props: {
    tabs: {
      type: Array,
      default: () => [],
    },
  },
  setup(props) {
    const rootTabs = inject('rootTabs', { props: {} })
    const instance = getCurrentInstance()

    const getBarStyle = () => {
      let style = {}
      let offset = 0
      let tabSize = 0

      const sizeName = ['top', 'bottom'].indexOf(rootTabs.props.tabPosition) !== -1 ? 'width' : 'height'
      const sizeDir = sizeName === 'width' ? 'x' : 'y'

      props.tabs.every((tab) => {
        let $el = instance.parent.refs?.[`tab-${tab.setupState.paneName}`]
        if (!$el) { return false }
        if (!tab.setupState.active) {
          offset += $el[`client${firstUpperCase(sizeName)}`]
          return true
        } else {
          tabSize = $el[`client${firstUpperCase(sizeName)}`]

          const tabStyles = window.getComputedStyle($el)

          if (sizeName === 'width' && props.tabs.length > 1) {
            tabSize -= parseFloat(tabStyles.paddingLeft) + parseFloat(tabStyles.paddingRight)
          }
          if (sizeName === 'width') {
            offset += parseFloat(tabStyles.paddingLeft)
          }
          return false
        }
      })

      const transform = `translate${firstUpperCase(sizeDir)}(${offset}px)`
      style[sizeName] = tabSize + 'px'
      style.transform = transform
      style.msTransform = transform
      style.webkitTransform = transform

      return style
    }

    const barStyle = ref(getBarStyle())

    watch(() => props.tabs, () => {
      nextTick(() => {
        barStyle.value = getBarStyle()
      })
    })

    return {
      rootTabs,
      barStyle,
    }
  },
})
</script>
