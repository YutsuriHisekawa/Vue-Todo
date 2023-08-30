<script setup>
import { computed, reactive, ref } from 'vue';
import TodoCard from '@/components/TodoCard.vue';
import FormInput from '@/components/Form/Input.vue';
import { useForm } from 'vee-validate';
import { object, string } from 'yup';

const { handleSubmit, handleReset } = useForm({
  validationSchema: object().shape({
    todo: string()
      .required(' Di Isi dikek mas broo kolom e')
      .test('unique', '${path} wis onok isine', (value) => {
        return !todos.value.map((t) => t.text).includes(value);
      }),
  }),
});

const todos = ref([]);
let id = 0;
const incompleteTodos = computed(() => {
  return todos.value.filter((x) => x.completed === false);
});

const completedTodos = computed(() => {
  return todos.value.filter((x) => x.completed === true);
});

const onSubmit = handleSubmit((values) => {
  const newTodo = {
    id: ++id,
    text: values.todo,
    completed: false,
  };
  todos.value = [newTodo, ...todos.value];
  handleReset();
});

const remove = (todoItem) => {
  todos.value = todos.value.filter((t) => t.id !== todoItem.id);
};
</script>

<template>
  <main class="text-white overflow-y-scroll h-screen flex items-center justify-center bg-black">
    <main class="h-screen flex items-center justify-center px-4 md:px-0">
      <div class="w-full md:w-[50rem] max-w-xs md:max-w-3xl rounded overflow-hidden shadow-lg">
        <form
          class="mb-10 text-black"
          @submit.prevent="onSubmit"
          novalidate>
          <FormInput
            name="todo"
            placeholder=" What ?" />
        </form>
        <div class="grid grid-cols-2 md:grid-cols-12 gap-4">
          <div class="col-span-6">
            <TodoCard
              v-for="todoItem in incompleteTodos"
              :key="todoItem.id"
              :todo="todoItem"
              @remove="remove" />
          </div>
          <div class="col-span-6">
            <TodoCard
              v-for="todoItem in completedTodos"
              :key="todoItem.id"
              :todo="todoItem"
              @remove="remove" />
          </div>
        </div>
      </div>
    </main>
  </main>
</template>
