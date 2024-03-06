<template>
  <div id="container">
    <div class="CmsContent">
      <div class="rightContent">
        <Location :Homepage="$t('Home.Home')" :Title="content.Title"></Location>

        <!-- 联络我们页面 -->
        <div v-if="content.Key=='ContactUs'">
          <div class="maps">
            <div v-html="mapscontent.Body"></div>
          </div>
          <div class="left">
            <h1 class="CmsContentTitle">{{content.Title}}</h1>
            <div v-html="content.Body"></div>
          </div>
          <div class="right">

          </div>

          <!-- <hr> -->

          <div class="Form">
            <RNPForm formKey="ContactUs" />
          </div>
        </div>

        <!-- 其他页面 -->
        <div v-else>
          <h1 class="CmsContentTitle">{{content.Title}}</h1>
          <p v-html="content.Body"></p>
          <div v-if="content.Id===30973">
            <ins-calendar />
          </div>
        </div>
      </div>
      <div class="clear"></div>
    </div>
  </div>
</template>
<script lang="ts">
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';
@Component({
  components: {
    Location: () => import('@/components/base/InsLocation.vue'),
    RNPForm: () => import('@/views/regNPay/regNPayForm.vue'),
    InsCalendar: () => import('@/components/business/home/InsCalendar.vue')
  }
})
export default class InsCmsContent extends Vue {
  content: object={};
  mapscontent: object={};
  contentName:string='';

  // 获取关于我们cms内容
  getContent () {
    this.$Api.cms.getContentByDevice({ ContentId: this.id, IsMobile: this.isMobile }).then(result => {
      this.content = result.CMS;
      this.contentName = result.Category.Name;
      // this.$HiddenLayer();

      this.$nextTick(() => {
        if (result.CMS.Title) document.title = result.CMS.Title;
        (document.getElementsByName('keywords')[0] as any).content = result.CMS.SeoKeyword;
        (document.getElementsByName('description')[0] as any).content = result.CMS.SeoDesc;
        (document.getElementsByName('twitter:description')[0] as any).content = result.CMS.SeoDesc;
        (document.getElementsByName('twitter:title')[0] as any).content = result.CMS.Title;
      });
    });
  }
  getMaps () {
    this.$Api.cms.getContentByDevice({ key: 'maps', IsMobile: this.isMobile }).then(result => {
      this.mapscontent = result.CMS;
    });
  }

  get id () {
    return this.$route.params.id;
  }

  get isMobile () {
    return this.$store.state.isMobile;
  }

  mounted () {
    this.getContent();
    this.getMaps();
  }

  @Watch('isMobile', { deep: true })
  onMediaChange () {
    this.getContent();
    this.getMaps();
  }

  @Watch('id', { deep: true })
  onKeyChange () {
    this.content = {};
    this.getContent();
    this.getMaps();
  }
}
</script>
<style scoped lang="less">
.pc {
  .CmsContent {
    width: 1200px;
    margin: 0 auto;
    margin-top: 30px;
    margin-bottom: 30px;
  }
  .CmsContentTitle {
    font-size: 30px;
    font-weight: 700;
    margin-bottom: 50px;
    text-align: center;
  }
  .rightContent {
    // width: 70%;
    // float: left;
    // position: relative;
    margin-bottom: 30px;
    min-height: 700px;
    .maps{
      margin-bottom: 50px;
    }
    .left{
      margin-bottom: 15px;
      /deep/ p{
        text-align: center;
      }
    }
    /deep/ p{
      table{
        width: 100%;
        &:nth-child(1){
          tr{
            td:nth-child(1){
              width: 70%;
              display: table-cell;
            }
            td:nth-child(2){
              width: 30%;
              display: table-cell;
              img{
                display: block;
                width: 100%;
              }
            }
          }
        }
        &:nth-child(2){
          display: block;
          margin: 20px auto;
          tr{
            td{
              width: 100%;
              display: block;
            }
          }
        }
        &:nth-child(3){
          tr{
            td:nth-child(1){
              width: 58%;
              display: table-cell;
              padding-right: 2%;
              box-sizing: border-box;
            }
            td:nth-child(2){
              width: 40%;
              display: table-cell;
              img{
                display: block;
                width: 100%;
              }
            }
          }
        }
      }
    }
  }

  .Form {
    width: 80%;
    margin: 0 auto;

    /deep/ .RNPForm.default {
      width: 100%;
      // background-color: #fff;
      .FormMain {
        min-width: auto;

        #content {
          .btn.save {
            border: 0;
            width: 100%;
            background-color: #C3B44F;
            border-radius: 0;
          }

          #Anwers {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;

            .form-group {
              padding: 0;
              border: 0;
              width: 100%;
              .control-label{
                display: none;
              }
              .control-label-remark {
                display: none;
              }

              ::-webkit-input-placeholder{
                color: #000000;
              }
              ::-moz-placeholder{   /* Mozilla Firefox 19+ */
                color: #000000;
              }
              :-moz-placeholder{    /* Mozilla Firefox 4 to 18 */
                color: #000000;
              }
              :-ms-input-placeholder{  /* Internet Explorer 10-11 */
                color: #000000;
              }

              fieldset {
                &.text {
                  input[type="text"] {
                    border: 1px solid #ccc;
                    padding: 10px 15px;
                    font-size: 15px;
                    box-sizing: border-box;
                  }
                }

                &.email {
                  input[type="email"] {
                    border: 1px solid #ccc;
                    padding: 10px 15px;
                    font-size: 15px;
                    box-sizing: border-box;
                  }
                }

                &.textarea {
                  textarea {
                    height: 165px;
                    border: 1px solid #ccc;
                    padding: 15px;
                    font-size: 15px;
                    box-sizing: border-box;
                  }
                }
              }
            }
          }
        }

        #preview {
          padding: 0;
        }
      }
    }
  }

  .clear {
    clear: both;
  }

}

.mobile {
  .CmsContent{
    background-color: #f2f1e2;
  }
  .CmsContentTitle {
    font-size: 18px;
    font-weight: 700;
    margin-bottom: 30px;
    text-align: center;
    margin-top: 30px;
  }
  .rightContent {
    width: 90%;
    margin: 0 auto;
    position: relative;
    padding-bottom: 3rem;
    padding-top: 1rem;
    /deep/ p{
      font-size: 1.2rem;
      line-height: 1.8rem;
      color: rgb(51, 51, 51);
      text-align: justify;
    }
    /deep/ h4{
      color: #333;
    }
  }

  .Form {
    /deep/ .RNPForm.default {
      background-color: transparent;
      .FormMain {
        min-width: auto;

        #content {
          .btn.save {
            border: 0;
            width: 100%;
            background-color: @base_color;
            border-radius: 0;
            font-size: 1.3rem;
            height: 3rem;
            margin: 0;
            font-weight: bold;
          }

          #Anwers {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;

            .form-group {
              padding: 0;
              border: 0;
              width: 100%;
              .control-label{
                display: none;
              }
              .control-label-remark {
                display: none;
              }

              ::-webkit-input-placeholder{
                color: #000000;
              }
              ::-moz-placeholder{   /* Mozilla Firefox 19+ */
                color: #000000;
              }
              :-moz-placeholder{    /* Mozilla Firefox 4 to 18 */
                color: #000000;
              }
              :-ms-input-placeholder{  /* Internet Explorer 10-11 */
                color: #000000;
              }

              fieldset {
                &.text {
                  input[type="text"] {
                    border: 1px solid #ccc;
                    padding: 0.8rem 1.3rem;
                    font-size: 1rem;
                    box-sizing: border-box;
                  }
                }

                &.email {
                  input[type="email"] {
                    border: 1px solid #ccc;
                    padding: 0.8rem 1.3rem;
                    font-size: 1rem;
                    box-sizing: border-box;
                  }
                }

                &.textarea {
                  textarea {
                    height: 9rem;
                    border: 1px solid #ccc;
                    padding: 1.3rem;
                    font-size: 1rem;
                    box-sizing: border-box;
                  }
                }
              }
            }
          }
        }

        #preview {
          padding: 0;

          input[type="button"] {
            font-size: 1.1rem;
            height: 3.2rem;

            &:first-of-type {
              margin-top: 2rem;
            }
          }
        }
      }
    }
  }

  .clear {
    clear: both;
  }
}
</style>
