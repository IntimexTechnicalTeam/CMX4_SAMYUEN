<template>
  <div class="about-main">
    <div class="main_1 fix">
      <p class="title">{{content.Title}}</p>
      <div class="content fix">
        <div class="left">
          <div class="body" v-html="content.Body"></div>
        </div>
        <div class="right">
          <RNPForm formKey="ContactUs" />
        </div>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';

@Component({
  components: {
    RNPForm: () => import('@/views/regNPay/regNPayForm.vue')
  }

})
export default class CmxAboutUs extends Vue {
  content: object = {};
  // 获取关于我们cms内容
  getContent () {
    // this.$Api.cms.getContentByKey({ key: 'k_ab' }).then((result) => {
    //   this.content = result;
    // });

    this.$Api.cms.getContentByDevice({ key: 'ContactUs', IsMobile: this.isMobile }).then(result => {
      this.content = result.CMS;
    });
  }

  get isMobile () {
    return this.$store.state.isMobile;
  }

  mounted () {
    this.getContent();
  }

  @Watch('isMobile', { deep: true })
  onMediaChange () {
    this.getContent();
  }
}
</script>
<style lang="less">
.pc{
  .RNPForm {

      .content{
        .form-group{
          .control-label{
            display: none;
          }
        }
      }
    }
}
.mobile{
  .RNPForm {
      .content{
        .form-group{
          .control-label{
            display: none;
          }
        }
      }
    }
}

</style>
<style lang="less" scoped>
.pc {
    .about-main {
        width: 1200px;
        margin: 0 auto;
        .title{
          font-size: 30px;
          color: #000;
          margin-bottom: 50px;
          text-align: center;
          font-weight: bold;
        }
      .main_1{
        margin-top: 50px;
      }

        .content {

              .left{
                float: left;
                width: 50%;
                margin-bottom: 15px;
              }
              .right{
                float: right;
                width: 50%;
                margin-bottom: 15px;
                /deep/ .RNPForm{
                  width: 100% !important;
                }
              }
            /deep/ p {
                font-size: 18px;
                color: #363636;
                // line-height: 36px;
                text-align: left;

            }

        }

    }

}

.mobile {
    .about-main {
        margin: 2rem auto 2rem;
        padding: 0 0.8rem;
        box-sizing: border-box;
        .title {
            font-size: 1.8rem;
            color: #000;
            font-weight: bold;
            margin-bottom: 2rem;
            text-align: center;
            }
        .content {
            padding: 0 0.5rem;

            .body {
              /deep/ p {
                font-size: 1.3rem;
                color: #363636;
                line-height: 1.8rem;
                margin-bottom: 1rem;
            }
                  /deep/ .left{
                    width: 100%;
                   margin-bottom: 2rem;
                  text-align: center;
                  }
                  /deep/ .right{
                    width: 100%;
                    margin-bottom: 2rem;
    text-align: center;
                  }
                  /deep/ .maps{
                    iframe{
                      border: none;
                      display: block;
                    }
                  }
              }
        }

        .cover {
            margin-bottom: 3.5rem;
            text-align: center;
            img {
            max-width: 100%;
            }
        }
    }

}
</style>
