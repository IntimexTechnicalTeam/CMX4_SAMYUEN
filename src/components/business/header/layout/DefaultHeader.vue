<template>
    <div class="header-box" :class="{'ENG':$Storage.get('locale') === 'E'}">
      <div class="header-w">
        <div class="flex-box">
          <ins-logo />
          <ins-menu v-if="!isMobile" />
          <!-- <ins-menu :layout="1" /> -->
          <img class="slide-menu" src="/static/Images/nav-icon.png" @click="showSlideMenu" v-if="isMobile" />
        </div>
        <div v-if="!isMobile">

          <div class="absolute" v-click-outside="closeDialog">
            <!-- 搜索框开始 -->
          <div class="search-box">
            <input type="text" placeholder="Search ..." v-model="key" @keyup.enter="searchKeyChange" />
            <span class="searchBtn" @click="searchKeyChange"></span>
          </div>
          <div class="MemberLogin" v-show="!isLogin">
            <a href="/account/Register"><span class="Title" >{{$t('Register.BecomeMember')}}</span></a>
          </div>
          <div class="MemberLogin">
            <a href="javascript:;" @click.stop="menberCentral" :class="{'FontCn':currentlang!=='E'}"><span class="Title" v-if="!isLogin">{{$t('Login.MemberLogin')}}</span><span class="Title" v-else>{{UserName}}</span></a>
            <transition name="slide-fade">
              <div class="lang_flow" v-show="showMenberCentral" @click="memberCentral">
                <div @click="goUrl('/account/memberInfo')" class="ii">{{$t('MemberInfo.MemberInfoTitle')}}</div>
                <div @click="goUrl('/account/forgetPassword')"  class="ii">{{$t('Forgetpassword.ForgetPassword')}}</div>
                <div @click="logout()">{{$t('Account.Logout')}}</div>
              </div>
            </transition>
          </div>
          <ins-lang-switch v-if="!isMobile" />
        </div>
        </div>

      </div>

    </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import sdk from '@/sdk/InstoreSdk';
import Cookie from 'js-cookie';
import storage from '@/sdk/common/Storage';
import Auth from '@/sdk/common/Auth';
@Component({
  components: {
    InsLogo: () => import('@/components/base/InsLogo.vue'),
    InsLangSwitch: () => import('@/components/business/header/InsLangSwitch.vue'),
    InsMenu: () => import('@/components/business/header/InsMenu.vue')
  }
})
export default class DefaultHeader extends Vue {
  @Prop() private showInFixed!: boolean;

  key: string = '';
  private showMenberCentral:boolean = false;
  UserName:string='';
  isShowSearch: boolean = false;

  closeDialog () {
    this.showMenberCentral = false;
  }
  goUrl (val) {
    window.location.href = val;
    this.showMenberCentral = false;
  }
  menberCentral () {
    if (!this.$Storage.get('isLogin')) {
      window.location.href = '/account/login';
    } else {
      this.showMenberCentral = !this.showMenberCentral;
    }
  }
  memberCentral (e) {
    let target = e.target as HTMLElement;
    if (target.className === 'ii' && target.dataset.to) {
      this.$router.push({
        path: target.dataset.to
      });
    }
  }
  showSlideMenu () {
    let isShow = !JSON.parse(JSON.stringify(this.menuShow));
    this.$store.dispatch('isShowMenu', isShow);
  }

  logout () {
    let _this = this;
    sdk.api.member.logout().then(
      function (data) {
        if (data) _this.$message(_this.$t('Message.SucceedInOperating') as string);
        _this.$store.dispatch('Logout');
        window.location.href = '/';
        Cookie.remove('access_token');
        Auth.GetToken();
        _this.showMenberCentral = false;
      }
    );
  }
  getAccount () {
    let _this = this;
    sdk.api.member.getAccount().then(
      function (data) {
        if (data.Logined) {
          _this.$store.dispatch('setUser', (data.FirstName + ' ' + data.LastName).toUpperCase());
          _this.$store.dispatch('doLogin');
          _this.UserName = data.FirstName + ' ' + data.LastName;
        } else {
          _this.$store.dispatch('Logout', true);
        }
      },
      function (data) {
        _this.$message({
          message: data,
          type: 'error',
          customClass: 'messageboxNoraml'
        });
      }
    );
  }

  searchKeyChange () {
    // alert('search!');
    this.$store.dispatch('isShowMenu', false);
    this.$router.push({
      path: '/cms/search',
      name: 'search',
      params: {
        key: this.key
      }
    });
  }

  get menuShow () {
    return this.$store.state.isShowMenu;
  }
  get currentlang () {
    return this.$i18n.locale;
  }

  get isMobile () {
    return this.$store.state.isMobile;
  }
  get isLogin () {
    return this.$store.state.isLogin;
  }
  mounted () {
    this.getAccount();
  }
}
</script>

<style scoped lang="less">
.pc {
    .header-box {
      // background-color: #C3B44F;
      width: 100%;
      min-width:1100px;
      margin: 0 auto;
      position: relative;
      .header-w{
        width: 1200px;
        margin: 0 auto;
        position: relative;
      }
      .flex-box {

        display: flex;
        justify-content: space-between;
        align-items: end;
        // min-height: 120px;
        // padding: 0 1.6% 0 4.2%;
        box-sizing: border-box;

        .logo {
          width:135px;
          // min-width: 170px;
        }
      }

      /deep/ .langSwitch {
        font-size: 17px;
        top: 10px;
        right: 20px;
        p {
          &.active {
            color: #d5ee1f;
            font-weight: bold;
          }
        }
      }
      .absolute{
        position: absolute;
        top: 10px;
        right: 20px;
        display: flex;
        justify-content: space-between;
      }

      /deep/ .nav_menu {
        // padding-top: 70px;
        // max-width: 75%;
        background-color: #C3B44F;
        width: 1000px;
        >ul {
          justify-content: space-between;
          >li {
            width: auto;
            padding: 18px 32px;

            a {
              color: #fff;
              font-size: 18px;
              padding: 0;
            }

            > ul {
              left: calc(50% - 120px);
            }

            ul {
              width: 240px;
              border: 0;
              box-shadow: 0 0 10px 0 @base_color;
              li {
                > a {
                  padding: 15px;
                  color: #676767;
                  font-size: 16px;
                }

                &:hover {
                  background-color: @menu_hover;
                  > a {
                    color: #fff;
                  }
                }
              }
            }
          }
        }
      }
    }

    .search-box {
      box-sizing: border-box;
      display: inline-flex;
      align-items: center;
      border-radius: 0;
      overflow: hidden;
      margin: 0 8px;
      border: 1px solid @base_color;
      // background: #fff;
      // position: absolute;
      // right: 15%;
      // top: 10px;

      .searchBtn{
        width: 31px;
        height: 31px;
        display: inline-block;
        background: url(/static/Images/search.png) no-repeat center center;
        background-size: 60%;
        flex-shrink: 0;
        cursor: pointer;
        background-color: @base_color;
        // border-radius: 50%;
        // margin-left: 5px;
      }

      input {
        width: 142px;
        height: 28px;
        font-size: 14px;
        color: #C3B44F;
        appearance: none;
        -webkit-appearance: none;
        -moz-appearance: none;
        -ms-appearance:none;
        outline: none;
        border: 0;
        background-repeat: no-repeat;
        background-position-y: 4px;
        padding: 0 15px;
        box-sizing: border-box;
        background-color: transparent;
        border-radius: 30px;
        // border: 1px solid @base_color;

        &:focus {
          border-color: @base_color;
        }
      }

      input::-webkit-input-placeholder{
        color: #666;
      }
      input::-moz-placeholder{   /* Mozilla Firefox 19+ */
        color: #666;
      }
      input:-moz-placeholder{    /* Mozilla Firefox 4 to 18 */
        color: #666;
      }
      input:-ms-input-placeholder{  /* Internet Explorer 10-11 */
        color: #666;
      }
    }
    .header-box.ENG{
      .header-w{
        .flex-box{
          /deep/ .nav_menu{
            li{
              padding: 18px 17px;
            }
          }
        }
      }
    }
.MemberLogin {
          border: 1px solid #C3B44F;
          padding: 6px 10px;
          position: relative;
          margin-left: 5px;
          margin-right: 5px;
        &::after{
            height: 40%;
            width: 0px;
            content: '';
            position: absolute;
            right: 0px;
            background: #fff!important;
            top: 30%;
          }
        .lang_flow{
          position: absolute;
          top: 50px;
          left: 50%;
          transform: translateX(-50%);
          width: 200px;
          background: #FFF;
          z-index: 999;
          box-shadow: 0 0 3px #ccc;
          >div{
            color:#000;
            font-size: 16px;
            height: 3rem;
            display: flex;
            justify-content: center;
            align-items: center;
            border-bottom: 1px solid #eee;
            padding-left: 10px;
            padding-right: 10px;
            cursor: pointer;
            &:first-child {
              border-top: 1px solid #eee;
            }
          }
        }
          a {
            width: 100%;
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            justify-content: center;
          }
        .img {
          display: flex;
          align-items: center;
          justify-content: center;
          margin-right: 8px;
          img {
            width: 20px;
          }
        }
        .Title {
          display: flex;
          align-items: center;
          justify-content: center;
          font-size: 14px;
          color: #C3B44F;
        }
      }
      li{
        width: calc(25% - 1px);
        /* float: left; */
        margin-bottom: 5px;
        text-align: center;
        display: flex;
        align-items: center;
        justify-content: center;
        position: relative;
        height: 48px;
        padding-top: 5px;
        padding-bottom: 5px;
        &::after{
          height: 40%;
          width: 2px;
          content: '';
          position: absolute;
          right: 0px;
          background: #ab8135;
          top: 30%;
        }
        &:nth-child(8){
        &::after{
            height: 40%;
            width: 2px;
            content: '';
            position: absolute;
            right: 0px;
            background: #fff!important;
            top: 30%;
          }
        }
        a{
          color: #02499b;
          font-size: 14px;
          text-align: center;
          padding-left: 8px;
          padding-right: 8px;
          display: block;
        }
      }
}

.mobile {
    .header-box {
      .flex-box {
        height: 7rem;
        background-color: #f2f1e2;
        position: relative;
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 0 1.5rem;
        box-shadow: 0 0 5px #B8B38B;
        z-index: 10;
        .logo {
          width: 6rem;
        }

        .slide-menu {
          cursor: pointer;
        }
      }

      .search-box {
        box-sizing: border-box;
        display: flex;
        align-items: center;
        border-radius: 30px;
        overflow: hidden;
        margin: 0 8px;
        background: #fff;

        .searchBtn{
          width: 25px;
          height: 25px;
          display: inline-block;
          background: url(/static/Images/search.png) no-repeat center center;
          background-size: 40%;
          flex-shrink: 0;
          cursor: pointer;
          background-color: @base_color;
          border-radius: 50%;
          margin-left: 5px;
        }

        input {
          width: 142px;
          height: 23px;
          font-size: 9px;
          color: @font_color;
          appearance: none;
          -webkit-appearance: none;
          -moz-appearance: none;
          -ms-appearance:none;
          outline: none;
          border: 0;
          background-repeat: no-repeat;
          background-position-y: 4px;
          padding: 0 15px;
          box-sizing: border-box;
          background-color: transparent;
          border-radius: 30px;
          border: 1px solid @base_color;

          &:focus {
            border-color: @base_color;
          }
        }

        input::-webkit-input-placeholder{
          color: #666;
        }
        input::-moz-placeholder{   /* Mozilla Firefox 19+ */
          color: #666;
        }
        input:-moz-placeholder{    /* Mozilla Firefox 4 to 18 */
          color: #666;
        }
        input:-ms-input-placeholder{  /* Internet Explorer 10-11 */
          color: #666;
        }
      }
    }
}
</style>
