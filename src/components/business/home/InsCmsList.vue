<template>
  <div class="cms-list">
    <p class="catName">
        <!-- {{$t('Home.ourservice')}} -->
        {{contentname}}
    </p>
    <div v-if="!isMobile">
        <swiper class="swiperPC" :options="swiperOptionCMS" ref="mySwiper" v-if="contents.length">
        <!-- slides -->
        <swiper-slide v-for="(cms,index) in contents" :key="index">
            <router-link :to="'/CMS/content/' + cms.Id">
                <div class="cover">
                    <img :src="cms.Cover" />
                </div>
                <div class="info">
                    <p class="title">{{cms.Title}}</p>
                    <p class="intro" v-html="cms.Body"></p>
                </div>
            </router-link>
        </swiper-slide>
        <div class="swiper-scrollbar"></div>
        </swiper>
    </div>
    <div v-else>
        <swiper class="swiperMobile" :options="swiperOptionM" ref="mySwiper" v-if="contents.length">
        <!-- slides -->
        <swiper-slide v-for="(cms,index) in contents" :key="index">
            <router-link :to="'/CMS/content/' + cms.Id">
                <div class="cover">
                    <img :src="cms.Cover" />
                </div>
                <div class="info">
                    <p class="title">{{cms.Title}}</p>
                    <p class="intro" v-html="cms.Body"></p>
                </div>
            </router-link>
        </swiper-slide>
        <div class="swiper-scrollbar"></div>
        </swiper>
    </div>
    <router-link class="view-more" to="/cms/catDetail/50236">{{$t('Action.ViewMore')}}</router-link>
    <!-- <el-row :gutter="isMobile ? 20 : 58" class="list">
        <el-col :span="12" :sm="6" v-for="(cms,index) in contents" :key="index">
            <router-link :to="'/CMS/content/' + cms.Id">
                <div class="cover">
                    <img :src="cms.Cover" />
                </div>
                <div class="info">
                    <p class="title">{{cms.Title}}</p>
                    <p class="intro" v-html="cms.Body"></p>
                </div>
            </router-link>
        </el-col>
    </el-row> -->
  </div>
</template>
<script lang="ts">
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';
import { swiper, swiperSlide } from 'vue-awesome-swiper/src';
@Component({
  components: {
    swiper,
    swiperSlide
  }
})
export default class InsCmsList extends Vue {
  @Prop({ default: '' }) private catKey!: string; // key值

  contents: object[] = [];
  contentname:string = '';
  swiperOptionCMS: object = {
    slidesPerView: 4,
    spaceBetween: 30,
    scrollbar: {
      el: '.swiper-scrollbar',
      hide: true
    }
  };
  swiperOptionM: object = {
    slidesPerView: 2,
    spaceBetween: 10,
    scrollbar: {
      el: '.swiper-scrollbar',
      hide: true
    }
  };

  // 获取关于cms内容列表
  //   getContents () {
  //     var page = 0;
  //     if (this.isMobile) {
  //       page = 2;
  //     } else {
  //       page = 4;
  //     }
  //     this.$Api.cms.getContentsByCatKeyEx({ Key: this.catKey, Page: 1, PageSize: page, IsMobile: this.isMobile }).then((result) => {
  //       this.contents = result.Data;
  //       console.log(result, 'getContentsByCatKeyEx');
  //     });
  //   }

  get isMobile () {
    return this.$store.state.isMobile;
  }

  getCategoryByDevice () {
    this.$Api.cms
      .getCategoryByDevice({ Key: this.catKey, IsMobile: this.isMobile })
      .then(result => {
        this.contents = result.Contents;
        this.contentname = result.Name;
        console.log(result, 'getCategoryBy');
      });
  }

  mounted () {
    // this.getContents();
    this.getCategoryByDevice();
  }

  @Watch('isMobile', { deep: true })
  onMediaChange () {
    // this.getContents();
    this.getCategoryByDevice();
  }
}
</script>
<style lang="less">
    .pc{
        .swiperPC{
                margin-bottom: 50px;
                .swiper-slide{
.cover {
                    margin-bottom: 15px;
                    img {
                        width: 100%;
                        height: 100%;
                        object-fit: cover;
                        display: block;
                        object-position: 50% 50%;
                    }
                }

                .info {
                    .title {
                        color: #6F5E0C;
                        font-weight: bold;
                        font-size: 18px;
                        margin-bottom: 15px;
                        text-overflow: -o-ellipsis-lastline;
                        overflow: hidden;
                        text-overflow: ellipsis;
                        display: -webkit-box;
                        -webkit-line-clamp: 2;
                        line-clamp: 2;
                        -webkit-box-orient: vertical;
                    }

                    .intro {
                         p{
font-size: 14px;
                        color: #444444;
                        line-height: 24px;
                        text-overflow: -o-ellipsis-lastline;
                        overflow: hidden;
                        text-overflow: ellipsis;
                        display: -webkit-box;
                        -webkit-line-clamp: 3;
                        line-clamp: 3;
                        -webkit-box-orient: vertical;
                        }

                    }
                }
                }

            }
            .view-more{
                font-size: 18px;
                color: #6F5E0C;
                padding: 12px 40px;
                border: 1px solid #6F5E0C;
                margin-top: 20px;
                display: table;
                text-align: center;
                margin-left: auto;
                margin-right: auto;
                border-radius: 10px;
            }
    }
    .mobile{
        .swiperMobile{
                margin-bottom: 2rem;
                list-style: none;
                .swiper-slide{
                    .cover {
                    margin-bottom: 0.5rem;
                    img {
                        max-width: 100%;
                        max-height: 100%;
                    }
                }

                .info {
                    .title {
                        font-size: 1.2rem;
                        color: #6F5E0C;
                        margin-bottom: 0.5rem;
                        font-weight: bold;
                        overflow: hidden;
                        display: -webkit-box;
                        -webkit-line-clamp: 2;
                        -webkit-box-orient: vertical;
                        word-wrap: break-word;
                        // letter-spacing: 0.5rem;

                        &.en {
                            letter-spacing: unset;
                        }
                    }

                    .intro {
                        font-size: 1rem;
                        color: #333;
                        overflow: hidden;
                        display: -webkit-box;
                        -webkit-line-clamp: 2;
                        -webkit-box-orient: vertical;
                        word-wrap: break-word;
                        min-height: 2.22rem;
                         p{
                            font-size: 1rem;
                        color: #333;
                        overflow: hidden;
                        display: -webkit-box;
                        -webkit-line-clamp: 2;
                        -webkit-box-orient: vertical;
                        word-wrap: break-word;
                        min-height: 2.22rem;
                        }
                    }
                }
                }

            }
            .view-more{
            border: 1px solid #6F5E0C;
            display: inline-block;
            font-size: 1.2rem;
            color: #6F5E0C;
            padding: 0.7rem 3.4rem;
            display: table;
            text-decoration: none;
            margin: 2rem auto;
            border-radius: 5px;
        }
    }
</style>
<style lang="less" scoped>
.pc {
    .cms-list {
        width: 1200px;
        margin: 0 auto;

        .catName {
            font-size: 30px;
            color: #000;
            margin-bottom: 30px;
            margin-top: 50px;
            text-align: center;
            font-weight: bold;
        }

        .list {
            display: flex;
            justify-content: space-between;
            flex-wrap: wrap;

            .el-col {
                margin-bottom: 50px;

                &:nth-child(3n) {
                    margin-right: 0;
                }

                .cover {
                    margin-bottom: 15px;
                    img {
                        width: 100%;
                        height: 100%;
                        object-fit: cover;
                        display: block;
                        object-position: 50% 50%;
                    }
                }

                .info {
                    .title {
                        color: #6F5E0C;
                        font-weight: bold;
                        font-size: 18px;
                        margin-bottom: 15px;
                        text-overflow: -o-ellipsis-lastline;
                        overflow: hidden;
                        text-overflow: ellipsis;
                        display: -webkit-box;
                        -webkit-line-clamp: 2;
                        line-clamp: 2;
                        -webkit-box-orient: vertical;
                    }

                    .intro {
                        font-size: 14px;
                        color: #444444;
                        line-height: 24px;
                        text-overflow: -o-ellipsis-lastline;
                        overflow: hidden;
                        text-overflow: ellipsis;
                        display: -webkit-box;
                        -webkit-line-clamp: 3;
                        line-clamp: 3;
                        -webkit-box-orient: vertical;
                    }
                }
            }

        }
    }
}

.mobile {
    .cms-list {
        padding: 0 1rem;
        .catName {
            font-size: 1.8rem;
            color: #000;
            font-weight: bold;
            text-align: center;
            margin-bottom: 2rem;
        }

        .list {
            display: flex;
            justify-content: space-between;
            flex-wrap: wrap;

            .el-col {
                margin-bottom: 2rem;
                list-style: none;

                &:nth-child(2n) {
                    margin-right: 0;
                }

                .cover {
                    margin-bottom: 0.5rem;
                    img {
                        max-width: 100%;
                        max-height: 100%;
                    }
                }

                .info {
                    .title {
                        font-size: 1.2rem;
                        color: #6F5E0C;
                        margin-bottom: 0.5rem;
                        font-weight: bold;
                        overflow: hidden;
                        display: -webkit-box;
                        -webkit-line-clamp: 2;
                        -webkit-box-orient: vertical;
                        word-wrap: break-word;
                        // letter-spacing: 0.5rem;

                        &.en {
                            letter-spacing: unset;
                        }
                    }

                    .intro {
                        font-size: 1rem;
                        color: #333;
                        overflow: hidden;
                        display: -webkit-box;
                        -webkit-line-clamp: 2;
                        -webkit-box-orient: vertical;
                        word-wrap: break-word;
                        min-height: 2.22rem;
                    }
                }
            }

        }
    }
}
</style>
