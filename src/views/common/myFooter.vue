<template>
  <div class="myFooter-wrap" v-show="showFooter">
    <div class="myFooter">
      <div class="icp" style="padding-bottom: 0px;">本网站由
        <a href="https://poetize.cn/" target="_blank">Poetize</a> 强力支持
      </div>

      <!-- <div class="icp">© 2023 成长画廊GrowthGallery
        <a href="http://beian.miit.gov.cn/" target="_blank">粤ICP备2023117589号</a>-1
      </div> -->
      <div class="icp">© 2024 成长走廊GrowthGallery
        <a href="http://beian.miit.gov.cn/" target="_blank">粤ICP备2023117589号</a>-2
      </div>
    </div>
  </div> 
</template>

<script>
export default {
  props: {
    showFooter: {
      type: Boolean,
      default: true
    }
  },
  data() {
    return {}
  },
  created() {
  }
}
</script>

<style scoped>
.myFooter-wrap {
  user-select: none;
  animation: hideToShow 2s;
}

.myFooter {
  border-radius: 1.5rem 1.5rem 0 0;
  background: #5b96d6;
  ;
  text-align: center;
  color: var(--white);
  background-size: 300% 300%;
  animation: gradientBG 10s ease infinite;
}

.footer-title {
  padding-top: 10px;
  font-size: 16px;
}

.icp,
.icp a {
  color: var(--maxGreyFont);
  font-size: 13px;
}

.icp {
  padding-top: 10px;
  padding-bottom: 10px;
}

.icp a {
  text-decoration: none;
  transition: all 0.3s;
}

.icp a:hover {
  color: var(--themeBackground);
}
</style>
