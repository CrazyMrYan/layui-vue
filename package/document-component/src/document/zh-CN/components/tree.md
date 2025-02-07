::: anchor
:::

::: title 基本介绍
:::

::: describe 树形组件一般用于展示具有层级关系的数据
:::

::: title 基础使用
:::

::: demo 使用 `lay-tree` 标签, 创建树形组件, @node-click 监听节点点击。

<template>
  <lay-tree
    :data="data"
    @node-click="handleClick"
  >
  </lay-tree>
</template>

<script setup>
import { ref } from 'vue';

const data = ref([{
	title: '一级1',
	id: 1,
	field: 'name1',
	checked: true,
	spread: true,
	children: [{
		title: '二级1-1 可允许跳转',
		id: 3,
		field: 'name11',
		href: 'https://www.layui.com/',
		children: [{
			title: '三级1-1-3',
			id: 23,
			field: '',
			children: [{
				title: '四级1-1-3-1',
				id: 24,
				field: '',
				children: [{
					title: '五级1-1-3-1-1',
					id: 30,
					field: ''
				},
				{
					title: '五级1-1-3-1-2',
					id: 31,
					field: ''
				}]
			}]
		},
		{
			title: '三级1-1-1',
			id: 7,
			field: '',
			children: [{
				title: '四级1-1-1-1 可允许跳转',
				id: 15,
				field: '',
				href: 'https://www.layui.com/doc/'
			}]
		},
		{
			title: '三级1-1-2',
			id: 8,
			field: '',
			children: [{
				title: '四级1-1-2-1',
				id: 32,
				field: ''
			}]
		}]
	},
	{
		title: '二级1-2',
		id: 4,
		spread: true,
		children: [{
			title: '三级1-2-1',
			id: 9,
			field: '',
			disabled: true
		},
		{
			title: '三级1-2-2',
			id: 10,
			field: ''
		}]
	},
	{
		title: '二级1-3',
		id: 20,
		field: '',
		children: [{
			title: '三级1-3-1',
			id: 21,
			field: ''
		},
		{
			title: '三级1-3-2',
			id: 22,
			field: ''
		}]
	}]
},
{
	title: '一级2',
	id: 2,
	field: '',
	spread: true,
	children: [{
		title: '二级2-1',
		id: 5,
		field: '',
		spread: true,
		children: [{
			title: '三级2-1-1',
			id: 11,
			field: ''
		},
		{
			title: '三级2-1-2',
			id: 12,
			field: ''
		}]
	},
	{
		title: '二级2-2',
		id: 6,
		field: '',
		children: [{
			title: '三级2-2-1',
			id: 13,
			field: ''
		},
		{
			title: '三级2-2-2',
			id: 14,
			field: '',
			disabled: true
		}]
	}]
},
{
	title: '一级3',
	id: 16,
	field: '',
	children: [{
		title: '二级3-1',
		id: 17,
		field: '',
		fixed: true,
		children: [{
			title: '三级3-1-1',
			id: 18,
			field: ''
		},
		{
			title: '三级3-1-2',
			id: 19,
			field: ''
		}]
	},
	{
		title: '二级3-2',
		id: 27,
		field: '',
		children: [{
			title: '三级3-2-1',
			id: 28,
			field: ''
		},
		{
			title: '三级3-2-2',
			id: 29,
			field: ''
		}]
	}]
}]);

function handleClick(node) {
 clickNode.value = node
}
</script>

:::

::: title 选择节点
:::

::: demo 使用 `showCheckbox` 属性开启复选框

<template>
  <lay-tree
    :data="data"
	v-model:checkedKeys="checkedKeys"
	:showCheckbox="showCheckbox"
  >
  </lay-tree>
</template>

<script setup>
import { ref } from 'vue';

const checkedKeys = ref([30,31])
const showCheckbox = ref(true)
</script>

:::

::: title 关闭连线
:::

::: demo 使用 `showLine` 属性关闭节点连线

<template>
  <lay-tree
    :data="data"
	:showLine="showLine"
  >
  </lay-tree>
</template>

<script setup>
import { ref } from 'vue';

const showLine=ref(false)
</script>

:::

::: title 过渡动画
:::

::: demo 使用 `collapse-transition` 属性开启展开过渡动画

<template>
  <lay-tree
    :data="data"
	collapse-transition
  >
  </lay-tree>
</template>

<script setup>
import { ref } from 'vue';
</script>

:::

::: title 定义标题
:::

::: demo 使用 `title` 插槽自定义节点标题

<template>
  <lay-tree
    :data="data"
	collapse-transition
  >	
	<template v-slot:title="{ data }">
		{{ data.id }}
	</template>
  </lay-tree>
</template>

<script setup>
import { ref } from 'vue';
</script>

:::

::: title Tree 属性
:::

::: table

| Name                             | Description                              | Accepted Values |
| -------------------------------- | ---------------------------------------- | --------------- |
| data                             | 树型组件数据,类型 TreeData \| TreeData[] | null            |
| showCheckbox                     | 是否显示复选框                           | false           |
| onlyIconControl                  | 是否仅允许节点左侧图标控制展开收缩       | false           |
| showLine                         | 是否开启连接线                           | true            |
| checkedKeys(v-model:checkedKeys) | 开启 showCheckbox 后, 选中的节点         | []              |
| collapse-transition              | 是否开启展示收起动画                     | false           |

:::

::: title Tree 数据
:::

::: table

| Name                | Description | Accepted Values |
|---------------------|-------------| --------------- |
| id                  | 唯一值         | -               |
| title               | 节点名称        | -               |
| children            | 子节点         | []              |
| disabled            | 该节点是否禁用     | false           |
| spread              | 该节点是否展开     | false           |

:::

::: title Tree 事件
:::

::: table

| Name       | Description     | Accepted Params |
| ---------- | --------------- | --------------- |
| node-click | 节点 click 事件 | --              |

:::

 

::: contributor transition
:::

::: previousNext transition
:::
