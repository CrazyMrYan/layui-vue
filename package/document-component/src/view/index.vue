<template>
  <div class="site-container">
    <div class="site-banner">
      <div class="site-banner-main">
        <div class="site-zfj site-zfj-anim">
          <img
            src="../assets/logo-png.png"
            style="width: 220px; border-radius: 10px"
          />
        </div>
        <div class="layui-anim site-desc site-desc-anim">
          <p class="web-font-desc">layui - vue</p>
          <cite>{{ t("home.description") }}</cite>
        </div>
        <div class="site-download">
          <router-link class="layui-inline site-down" to="/zh-CN/guide">
            Get Started
          </router-link>
          <a
            class="layui-inline site-down"
            href="javascript:void(0);"
            @click="changeTheme"
          >
            {{ appStore.theme === "dark" ? "Turn Off" : "Turn On" }}
          </a>
        </div>
        <div class="site-version">
          <span
            >{{ t("home.version") }}：<cite class="site-showv">
              {{ version }}
            </cite></span
          >
          <span
            >{{ t("home.download") }}：<em class="site-showdowns"
              >13,525</em
            ></span
          >
        </div>
        <div class="site-banner-other">
          <a
            href="https://gitee.com/layui-vue"
            target="_blank"
            rel="nofollow"
            class="site-star"
          >
            <i class="layui-icon"></i> Star <cite id="getStars">1215</cite>
          </a>
          <a
            href="https://gitee.com/layui-vue"
            target="_blank"
            rel="nofollow"
            class="site-fork"
          >
            Gitee
          </a>
          <a
            href="https://github.com/layui-vue"
            target="_blank"
            rel="nofollow"
            class="site-fork"
          >
            Github
          </a>
        </div>
      </div>
    </div>
    <div class="box-list">
      <lay-row :space="30">
        <lay-col :md="6" :sm="12" :xs="12">
          <div class="box">
            <h1 class="title">🌈 Classic design</h1>
            <p class="details">layui css.</p>
          </div>
        </lay-col>
        <lay-col :md="6" :sm="12" :xs="12">
          <div class="box">
            <h1 class="title">🐝 Small volume</h1>
            <p class="details">only 8.5 MB.</p>
          </div>
        </lay-col>
        <lay-col :md="6" :sm="12" :xs="12">
          <div class="box">
            <h1 class="title">🍬 Setup script</h1>
            <p class="details">use grammar sugar.</p>
          </div>
        </lay-col>
        <lay-col :md="6" :sm="12" :xs="12">
          <div class="box">
            <h1 class="title">😋 Quick build</h1>
            <p class="details">easy to use</p>
          </div>
        </lay-col>
      </lay-row>
    </div>
    <div class="footer footer-index">
      <p>Released under the <a href="/index.html">MIT License</a>.</p>
      <p>Copyright © 2021-2022 layui-vue.com</p>
    </div>
  </div>
</template>

<script>
import { inject } from "vue";
import { useI18n } from "vue-i18n";
import { useAppStore } from "../store/app";

export default {
  name: "index",
  setup() {
    const { t } = useI18n();
    const appStore = useAppStore();

    const changeTheme = () => {
      if (appStore.theme === "dark") {
        appStore.theme = "light";
      } else {
        appStore.theme = "dark";
      }
    };

    const version = inject("version");

    return {
      t,
      version,
      appStore,
      changeTheme,
    };
  },
};
</script>
<style>
#app,
html,
body {
  height: 100%;
}

.site-container {
  background: #fff;
  margin-top: 60px;
  height: 100%;
  width: 100%;
}

.site-banner {
  overflow: hidden;
  text-align: center;
  position: relative;
  height: calc(100% - 60px);
  background-image: url(../assets/background.svg);
  background-repeat: no-repeat;
  background-size: 100%;
}

.site-banner-main {
  display: flex;
  flex-direction: column;
  height: calc(100vh - 180px);
  justify-content: center;
  position: relative;
  text-align: center;
}

.site-download {
  margin-top: 72px;
  font-size: 0;
}

.site-download a {
  position: relative;
  padding: 0 20px 0 20px;
  height: 40px;
  line-height: 40px;
  border-radius: 4px;
  background: #f1f1f1;
  border: 1px solid #f1f1f1;
  font-size: 16px;
  color: #476582;
  font-weight: 500;
  font-family: Inter, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto,
    Oxygen, Ubuntu, Cantarell, "Fira Sans", "Droid Sans", "Helvetica Neue",
    sans-serif;
  transition: all 0.5s;
  -webkit-transition: all 0.5s;
  letter-spacing: 0.2px;
}

.site-download a:hover {
  border-radius: 10px;
}

.site-download a:first-child {
  background: #009688;
  color: white;
}

.site-download a + a {
  margin-left: 20px;
}

.site-zfj {
  padding-top: 25px;
  height: 220px;
}

.site-zfj-anim i {
  -webkit-animation-name: site-zfj;
  animation-name: site-zfj;
  -webkit-animation-duration: 5s;
  animation-duration: 5s;
  -webkit-animation-timing-function: linear;
  animation-timing-function: linear;
}

.site-zfj i {
  position: absolute;
  left: 50%;
  top: 50px;
  width: 200px;
  height: 200px;
  margin-left: -100px;
  font-size: 180px;
  color: #c2c2c2;
}

.site-desc-anim {
  -webkit-animation-name: site-desc;
  animation-name: site-desc;
}

.site-desc {
  position: relative;
  height: 70px;
  margin-top: 20px;
}

.site-desc .web-font-desc {
  color: #fff;
  color: #213547;
  font-size: 52px;
  opacity: 0.82;
}

.web-font-desc {
  font-size: 76px;
  line-height: 1.25;
  font-weight: 900;
  letter-spacing: -1.5px;
  max-width: 960px;
  margin: 0 auto;
}

.site-desc cite {
  position: absolute;
  bottom: -40px;
  left: 0;
  width: 100%;
  color: rgba(60, 60, 60, 0.7);
  font-style: normal;
}

.site-version {
  position: relative;
  margin-top: 18px;
  color: rgba(60, 60, 60, 0.7);
  font-size: 12px;
}

.site-version span {
  padding: 0 3px;
}

.site-version * {
  font-style: normal;
}

.site-version a {
  color: rgba(60, 60, 60, 0.7);
  text-decoration: none;
  margin-top: -4px;
}

.site-banner-other {
  position: relative;
  left: 0;
  top: 40px;
  bottom: 90px;
  width: 100%;
  text-align: center;
  font-size: 0;
}

.box-list {
  padding: 50px 250px;
}

.box {
  border: 1px solid rgba(60, 60, 60, 0.12);
  border-radius: 12px;
  padding: 24px;
  height: 100%;
  background-color: #f9f9f9;
}

.box .title {
  line-height: 24px;
  font-size: 16px;
  font-weight: 600;
}

.box .details {
  padding-top: 8px;
  line-height: 24px;
  font-size: 14px;
  font-weight: 500;
  color: rgba(60, 60, 60, 0.7);
}

.site-container .layui-field-title {
  border-top: 1px solid #d2d2d2;
}

.site-banner-other a {
  display: inline-block;
  vertical-align: middle;
  height: 28px;
  line-height: 28px;
  margin: 0 6px;
  padding: 0 8px;
  border-radius: 4px;
  color: rgba(60, 60, 60, 0.7);
  border: 1px solid rgba(60, 60, 60, 0.7);
  font-size: 14px;
  transition: all 0.5s;
  -webkit-transition: all 0.5s;
}

.footer {
  width: 100%;
  padding: 30px 0px;
  line-height: 30px;
  text-align: center;
  border-top: 1px solid #eee;
  color: rgba(60, 60, 60, 0.7);
  font-weight: 300;
  font-size: 13.6px;
  background: #f9f9f9;
}
</style>
