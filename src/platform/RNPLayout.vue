<template>
  <div>

    <ins-header />

    <router-view></router-view>
    <ins-footer />

    <ins-sidebar />

    <ins-slide-menu :direction="'right'" v-if="isMobile">
      <ins-menu-layout />
    </ins-slide-menu>
    <!-- <ins-copyright /> -->
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
@Component({
  components: {
    // InsCopyright: () => import('@/components/business/footer/InsCopyright.vue')
    InsHeader: () => import('@/components/business/header/InsHeader.vue'),
    InsFooter: () => import('@/components/business/footer/InsFooter.vue'),
    InsSlideMenu: () => import('@/components/business/header/InsSlideMenu.vue'),
    InsMenuLayout: () => import('@/components/business/header/InsMenuLayout.vue'),
    InsSidebar: () => import('@/components/business/header/InsSidebar.vue')
  }
})
export default class RNPLayout extends Vue {

}
</script>

<style lang="less">
.pc {
  min-width: 1200px;
}
</style>
