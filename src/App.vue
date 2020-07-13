<template>
  <transition name="fade">
    <!--<router-view>-->
      <div id="app">
        <router-view v-if="isRouterAlive"></router-view>
      </div>
    <!--</router-view>-->
  </transition>
</template>

<script>
  export default {
    name: 'App',
    provide () {
      return {
        reload: this.reload
      }
    },
    data () {
      return {
        isRouterAlive: true
      }
    },
    methods: {
      reload () {
        this.isRouterAlive = false
        this.$nextTick(function () {
          this.isRouterAlive = true
        })
      }
    }
  }
</script>
