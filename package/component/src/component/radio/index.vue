<script lang="ts">
import { computed, inject } from "vue";
export default {
  name: "LayRadio",
};
</script>

<script setup lang="ts">
import "./index.less";

export interface LayRadioProps {
  modelValue?: string | boolean;
  disabled?: boolean;
  label?: string;
  name?: string;
}

const props = defineProps<LayRadioProps>();

const emit = defineEmits(["update:modelValue", "change"]);

const radioGroup: any = inject("radioGroup", {});

const isGroup = computed(() => {
  return radioGroup != undefined && radioGroup?.name === "LayRadioGroup";
});

const naiveName = computed(() => {
  if (radioGroup.naiveName) {
    return radioGroup.naiveName;
  } else {
    return props.name;
  }
});

const isChecked = computed({
  get() {
    if (isGroup.value) {
      return radioGroup.modelValue.value === props.label;
    } else {
      return props.modelValue === props.label;
    }
  },
  set(val) {
    if (isGroup.value) {
      radioGroup.modelValue.value = props.label;
    } else {
      if (val) {
        emit("change", props.label);
        emit("update:modelValue", props.label);
      }
    }
  },
});

const handleClick = function () {
  if (!props.disabled) {
    isChecked.value = !isChecked.value;
  }
};
</script>

<template>
  <span class="layui-radio">
    <input type="radio" :value="label" :name="naiveName" />
    <div
      class="layui-unselect layui-form-radio"
      :class="{
        'layui-form-radioed': isChecked,
        'layui-radio-disabled layui-disabled': disabled,
      }"
      @click.stop="handleClick"
    >
      <i v-if="isChecked" class="layui-anim layui-icon layui-anim-scaleSpring"
        >&#xe643;</i
      >
      <i
        v-else
        class="layui-anim layui-icon layui-anim-scaleSpring layui-form-radioed"
        >&#xe63f;</i
      >
      <span><slot></slot></span>
    </div>
  </span>
</template>
