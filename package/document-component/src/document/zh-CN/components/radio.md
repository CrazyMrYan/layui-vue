::: anchor
:::

::: title 基本介绍
:::

::: describe 用于在多个备选项中选中单个状态。
:::

::: title 基础使用
:::

::: demo 使用 `lay-radio` 标签, 创建一个单选框

<template>
    <lay-radio v-model="selected1" name="action" label="1">写作</lay-radio>
    <lay-radio v-model="selected1" name="action" label="2">画画</lay-radio>
    <lay-radio v-model="selected1" name="action" label="3">运动</lay-radio>
</template>

<script>
import { ref } from 'vue'

export default {
  setup() {

    const selected1 = ref("1");

    return {
        selected1
    }
  }
}
</script>

:::

::: title 禁用状态
:::

::: demo

<template>
    <lay-radio v-model="selected2" name="action" label="1">写作</lay-radio>
    <lay-radio v-model="selected2" name="action" label="2">画画</lay-radio>
    <lay-radio v-model="selected2" name="action" label="3">运动</lay-radio>
    <lay-radio v-model="selected2" name="action" label="4" :disabled="disabled">禁用</lay-radio>
</template>

<script>
import { ref } from 'vue'

export default {
  setup() {

    const disabled = ref(true);
    const selected2 = ref("1");

    return {
        disabled,
        selected2
    }
  }
}
</script>

:::

::: title 事件回调
:::

::: demo

<template>
    <lay-radio v-model="selected3" name="action" label="1" @change="change">写作</lay-radio>
    <lay-radio v-model="selected3" name="action" label="2" @change="change">画画</lay-radio>
    <lay-radio v-model="selected3" name="action" label="3" @change="change">运动</lay-radio>
</template>

<script>
import { ref } from 'vue'

export default {
  setup() {

    const selected3 = ref("1");
    const change = function( current ) {
        console.log("当前值:" + current)
    }
    return {
        selected3,
        change
    }
  }
}
</script>

:::

::: title 单选分组
:::

::: demo

<template>
    <lay-radio-group name="action" v-model="selected4" @change="change4">
      <lay-radio label="1">写作</lay-radio>
      <lay-radio label="2">画画</lay-radio>
      <lay-radio label="3">运动</lay-radio>
    </lay-radio-group>
</template>

<script>
import { ref } from 'vue'

export default {
  setup() {

    const selected4 = ref("1");
    const change4 = function( current ) {
        console.log("当前值:" + current)
    }
    return {
        selected4,
        change4
    }
  }
}
</script>

:::

::: title Radio 属性
:::

::: table

| 属性    | 描述          | 默认值 |
| ------- | ------------- | ------ |
| name    | 原始属性 name | --     |
| label   | 当前值        | --     |
| v-model | 选中值        | --     |

:::

::: title Radio 事件
:::

::: table

| 事件   | 描述     | 参数             |
| ------ | -------- | ---------------- |
| change | 切换事件 | current : 当前值 |

:::

::: title RadioGroup 属性
:::

::: table

| 属性    | 描述          | 默认值 |
| ------- | ------------- | ------ |
| v-model | 选中值        | --     |

:::

::: title RadioGroup 事件
:::

::: table

| 事件   | 描述     | 参数             |
| ------ | -------- | ---------------- |
| change | 切换事件 | current : 当前值 |

:::

::: contributor radio
:::  

::: previousNext radio
:::