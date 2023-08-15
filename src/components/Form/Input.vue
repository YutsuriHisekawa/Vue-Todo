<script setup>

import { useField } from 'vee-validate';
import { toRef } from "vue";

const props = defineProps ({

  name : {
    type: String,
    required: true,
  },

  type:{
    type: String,
    default: "text",
  },
  required: {
    type:Boolean,
    default: true,
  },
  placeholder: {
    type: String,
    default: undefined,
  },

  value:{
    type:[String,Number],
    default: "",
  },
  
})

const name = toRef(props, "name");

const {
errorMessage,
handleBlur,
handleChange,
value: inputValue,
} = useField(name, undefined , {
  initialValue: props.value,
})

</script>

<template>

<div>
  <input
@input="handleChange"
@blur="handleBlur"
:id="name"
:name="name"
:type="type"
:required="required"
:placeholder="placeholder"
:value="inputValue"
  class="p-2 border rounded border-gray-300 w-full block outline-none "
/>
<p v-if="errorMessage" class="mt-1 text-red-700 text-sm"> {{ errorMessage }}
</p>
</div>

</template>