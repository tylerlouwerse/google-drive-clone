<template>
  <form class="w-[600px] h-[80px] flex items-center">
    <TextInput
      type="text"
      class="block w-full mr-2"
      placeholder="Search for files and folders"
      autocomplete
      v-model="form.search"
    />
  </form>
</template>

<script setup>
import { useForm } from '@inertiajs/vue3';
import TextInput from '@/Components/TextInput.vue';

const form = useForm({
  search: '',
});
</script>