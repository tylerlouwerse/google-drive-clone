<template>
  <Modal :show="modelValue" max-width="sm" @show="onShow">
    <div class="p-6">
      <h2 class="text-lg font-medium text-gray-900">
        Create New Folder
      </h2>
      <div class="mt-6">
        <InputLabel for="folderName" value="Folder Name" class="sr-only" />
        <TextInput
          id="folderName"
          ref="folderNameInput"
          class="mt-1 block w-full"
          :class="form.errors?.name ? 'border-red-500 focus:border-red-500 focus:ring-red-500' : ''"
          type="text"
          placeholder="Folder Name"
          v-model="form.name"
          @keyup.enter="createFolder"
        />
        <InputError :message="form.errors?.name" class="mt-2" />
      </div>
      <div class="mt-6 flex justify-end">
        <SecondaryButton @click="closeModal">Cancel</SecondaryButton>
        <PrimaryButton
          class="ml-3"
          :class="{ 'opacity-25': form.processing }"
          :disable="form.processing"
          @click="createFolder"
        >Submit</PrimaryButton>
      </div>
    </div>
  </Modal>
</template>

<script setup>
import { ref, nextTick } from 'vue';
import Modal from '@/Components/Modal.vue';
import InputLabel from '@/Components/InputLabel.vue';
import TextInput from '@/Components/TextInput.vue';
import InputError from '@/Components/InputError.vue';
import SecondaryButton from '@/Components/SecondaryButton.vue';
import { useForm } from '@inertiajs/vue3';
import PrimaryButton from '../PrimaryButton.vue';

const { modelValue } = defineProps({
  modelValue: Boolean
})

const form = useForm({
  name: '',
});

const emit = defineEmits(['update:modelValue']);

const folderNameInput = ref(null)

const onShow = () => {
  nextTick(() => {
    folderNameInput.value.focus();
  });
}

const createFolder = () => {
  form.post(route('folder.create'), {
    preserveScroll: true,
    onSuccess: () => {
      closeModal();
      form.reset();

      // TODO: Show success message
    },
    onError: () => folderNameInput.value.focus(),
  });
}

const closeModal = () => {
  emit('update:modelValue', false);
  form.clearErrors();
  form.reset();
}
</script>