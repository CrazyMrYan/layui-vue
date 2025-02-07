<template>
  <div
    class="layui-scroll"
    :class="{ hide: data.winWidth < 500 }"
    :style="{ height: height }"
  >
    <div class="layui-scroll-y">
      <div ref="scrollRef" class="layui-scroll-wrap" @scroll="onMosewheel">
        <slot></slot>
      </div>
      <div
        ref="barRef"
        class="layui-scroll-track"
        :style="{
          backgroundColor: data.heightPre == 1 ? 'rgba(0,0,0,0)' : trackColor,
        }"
      >
        <div
          :style="{
            height: data.barHeight + 'px',
            width: thumbWidth + 'px',
            transform: 'translateY(' + data.translateY + 'px)',
            backgroundColor: data.heightPre == 1 ? 'rgba(0,0,0,0)' : thumbColor,
          }"
          class="layui-scroll-thumb"
          @mousedown.stop.prevent="moveStart"
        ></div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  name: "LayScroll",
};
</script>

<script lang="ts" setup>
import "./index.less";
import { toRefs, onMounted, nextTick, reactive, onUnmounted, ref } from "vue";

export interface LayScrollProps {
  height?: string;
  trackColor?: string;
  thumbColor?: string;
  thumbWidth?: number;
}

const props = withDefaults(defineProps<LayScrollProps>(), {
  height: "100%",
  trackColor: "rgba(0,0,0,0)",
  thumbColor: "#eeeeee",
  thumbWidth: 6,
});

const emit = defineEmits(["arrive"]);

const scrollRef = ref<HTMLElement | null>(null);
const barRef = ref<HTMLElement | null>(null);

const data = reactive({
  translateY: 0, // 滚动块平移的距离
  heightPre: 0, // 可视高度和内容高度比
  barHeight: 0, // 滑块高度
  winWidth: document.body.clientWidth, //初始化浏览器页面宽度
});

let time = null; // 定时器
let isMove = false; // 判断鼠标是否点击滑块（为松开）
let moveClientY = 0; // 鼠标点击滑块时，相对滑块的位置
let trackHeight = 0; // 滚动条轨道高度
let wrapHeight = 0; // 容器高度（可视高度）
let wrapContentHeight = 0; // 内容高度（可滚动内容的高度）

// 页面挂载后计算滚动条
onMounted(() => {
  monitorWindow(); //监听窗口尺寸
  monitorScrollBar(); //监听内容元素尺寸
  nextTick(() => {
    //dom渲染后
    calculationLength(); //初始化延迟更新滚动条
  });
});

// 页面卸载清除定时器
onUnmounted(() => {
  window.clearInterval(time);
  time = null;
});

// 监听页面尺寸改变计算滚动条
const monitorWindow = function () {
  let time; //定时器，防抖，窗口持续变化，延迟更新滚动条
  window.addEventListener("resize", () => {
    data.winWidth = document.body.clientWidth; //页面改变监听宽度控制移动端隐藏滚动条
    clearTimeout(time);
    time = setTimeout(() => {
      //页面宽度变化继续监听，如果小于500就关闭自定义滚动条
      // console.log("浏览器窗口变化更新滚动条");
      initScrollListner();
    }, 500);
  });
};

//监听内容元素尺寸变化
const monitorScrollBar = function () {
  var monitorUl = scrollRef.value.children[0];
  // var monitorDiv= document;    // 监听document
  let MutationObserver =
    window.MutationObserver ||
    // @ts-ignore
    window.WebKitMutationObserver ||
    // @ts-ignore
    window.MozMutationObserver;
  let observer = new MutationObserver(function (mutations) {
    // console.log("内容元素变化更新滚动条");
    initScrollListner();
  });

  // 监听子节点增加或者内容撑起的尺寸
  observer.observe(monitorUl, {
    attributes: true,
    childList: true,
  });
};

// 初始化延迟监听滚动条
const calculationLength = function () {
  // 直接执行initScrollListner函数，获取滚动条长度部准确
  // 因为页面渲染有延迟，获取dom元素需要延迟
  // 每间隔10毫秒更新滑块长度
  time = setInterval(() => {
    // 计算滚动条长度
    initScrollListner();
  }, 50);
  // 间隔500毫秒清除定时器，滑块缩短会有动画效果，时间可延长没有影响
  setTimeout(() => {
    window.clearInterval(time);
    time = null;
  }, 2000);
};

// 计算滚动条高度
const initScrollListner = function () {
  let scroll = scrollRef.value;
  let bar = barRef.value;
  // scroll有时候拿不到元素，要判断一下
  if (scroll) {
    wrapContentHeight = scroll.scrollHeight;
    wrapHeight = scroll.clientHeight;
    trackHeight = bar.clientHeight;
    // console.log(wrapContentHeight ,wrapHeight);
    // 容器高度 / 内容高度   100 150
    data.heightPre = wrapHeight / wrapContentHeight;
    // 滑动块的高度 根据 容器和内容的比  乘以 滚动轨道 计算出 滑动块的高度
    data.barHeight = data.heightPre * trackHeight;
  }
};

// 内容滚动时，计算滑块移动的距离
const onMosewheel = function (e) {
  // scrollTop页面顶部滚出的高度
  // offsetHeight页面可视区域高度
  // scrollHeight页面正文全文高度
  // data.translateY滚动块平移的距离
  data.translateY = e.target.scrollTop * data.heightPre;
  if (data.translateY == 0) {
    // 到达顶部
    arrive("top");
  } else if (
    e.target.scrollTop + e.target.offsetHeight ==
    e.target.scrollHeight
  ) {
    // 滚出高度 + 可视区域高度 == 内容高度
    arrive("bottom");
  }
};

// 到达顶部或者底部通知父级元素
const arrive = function name(tb) {
  emit("arrive", tb);
};

// 鼠标点击滑块时
const moveStart = function (e) {
  isMove = true;
  // clientY：当鼠标事件发生时，鼠标相对于浏览器（这里说的是浏览器的有效区域）y轴的位置
  // data.translateY 滚动块平移的距离
  // moveClientY 鼠标点击滑块时，相对滑块的位置
  moveClientY = e.clientY - data.translateY;
  moveTo(); //移动时
  moveEnd(); //鼠标松开时
};

// 鼠标移动，改变thumb的位置以及容器scrollTop的位置
const moveTo = function () {
  document.onmousemove = (e) => {
    // 移动时候判断是不是松开，松开就不在执行滑块移动操作
    if (isMove) {
      // 移动滑块时，判断时候到达顶部或者底部
      if (e.clientY - moveClientY > trackHeight - data.barHeight) {
        // 滑块到达  底部  就不在改变滑块translateY值
        data.translateY = trackHeight - data.barHeight;
      } else if (e.clientY - moveClientY < 0) {
        // 滑块到达  顶部  就不在改变滑块translateY值
        data.translateY = 0;
      } else {
        //改变滑块位置
        data.translateY = e.clientY - moveClientY;
      }
      // 计算出内容盒子滚出顶部的距离
      scrollRef.value.scrollTop = data.translateY / data.heightPre;
    }
  };
};

// 鼠标从滑块松开时，不在监听滑块移动操作
const moveEnd = function () {
  document.onmouseup = (e) => {
    if (isMove) {
      isMove = false;
    }
  };
};

let dataRef = toRefs(data);
</script>
