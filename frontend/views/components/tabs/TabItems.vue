<template lang='pug'>
  transition(:name='transitionName')
    div(v-show='isActive' class='tab-item')
      .tab-header
        button.is-icon.tab-back.has-background(
          aria-label='back'
          @click='$parent.open = !$parent.open'
        )
          i.icon-chevron-left(aria-hidden='true')

        h2.is-title-2.main-title {{ $parent.title }}

      .tab-body
        slot
</template>

<script>
export default {
  name: 'TabItem',
  data () {
    return {
      isActive: false,
      transitionName: null
    }
  },
  methods: {
    // Used by parent TabWrapper.vue
    changeTab (isActive, transitionName) {
      this.transitionName = transitionName
      this.isActive = isActive
    }
  },
  created () {
    this.$parent.tabItems.push(this)
  },
  beforeDestroy () {
    const index = this.$parent.tabItems.indexOf(this)
    if (index >= 0) {
      this.$parent.tabItems.splice(index, 1)
    }
  }
}
</script>

<style lang='scss' scoped>
@import "@assets/style/_variables.scss";

// Header
.tab-header {
  display: flex;
  justify-content: center;
  align-items: center;
  position: sticky;
  top: 0;
  height: 4.75rem;
  width: 100%;
  z-index: 3;
  background-color: $background;

  @include desktop {
    justify-content: start;
    background-color: transparent;
    position: relative;
  }
}

.main-title {
  @include desktop {
    margin: 2.5rem 0 0 27px;
  }
}

.tab-back {
  position: absolute;
  top: 1rem;
  left: 1rem;
  z-index: 1;

  @include desktop {
    display: none;
  }
}

.tab-body {
  display: flex;
  justify-content: center;
  padding-top: 1.5rem;

  @include desktop {
    justify-content: flex-start;
    padding-top: 0;
  }
}
</style>
