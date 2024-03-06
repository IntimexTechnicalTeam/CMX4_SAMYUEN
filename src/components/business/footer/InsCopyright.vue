<template>
    <div class="copyright">
        <p>© Copyright {{year}} SAM YUEN METAPHYSICS LIMITED </p>

        <a target="_blank" href="https://eventizer.hk/">
            <img src="/static/Images/footer-logo.png" />
        </a>
    </div>
</template>
<script lang="ts">
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';
@Component
export default class InsCopyright extends Vue {
    year: number = 0;

    created () {
      let date = new Date();
      this.year = date.getFullYear();
    }
}
</script>
<style lang="less" scoped>
.copyright {
    background-color: #B8B38B;
}

.pc {
    .copyright {
        text-align: center;
        padding: 20px 0;
        padding-top: 0;
        display: flex;
        justify-content: center;
    align-items: center;
        p {
            color: #fff;
            font-size: 14px;
            display: block;
            vertical-align: middle;
        }

        img {
            vertical-align: middle;
            margin-left: 12px;
            display: block;
            width: 60px;
        }
    }
}

.mobile {
    .copyright {
        padding: 0.6rem 1rem 1rem;
        text-align: center;
        display: flex;
        justify-content: center;

        p {
            display: inline-block;
            color: #fff;
            font-size: 1rem;
            vertical-align: middle;
        }

        img {
            vertical-align: middle;
            margin-left: 0.5rem;
            width: 3.5rem;
            display: block;
        }
    }
}
</style>
