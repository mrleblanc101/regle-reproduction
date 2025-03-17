<script setup lang="ts">
import Drawer from '@/components/Drawer.vue'
import JSONViewer from '@/components/JSONViewer.vue'
import { useRegle } from '@regle/core'
import { required } from '@regle/rules'
import { ref, type Ref } from 'vue'

const form: Ref<{
  frequency: number | null
}> = ref({
  frequency: null
})

const { r$ } = useRegle(
  form,
  { frequency: { required } }
);

const onSubmit = async () => {
  const { result } = await r$.$validate();
    if (result) {


      alert(form.value.frequency);
    }
};
</script>

<template>
  <div class="px-6 text-gray-900 antialiased">
    <div class="mx-auto max-w-xl py-12 md:max-w-4xl">
      <h2 class="text-2xl mb-8">Regle reproduction</h2>
      <form class="flex flex-col" @submit.prevent="onSubmit">
        <label>Your name</label>
        <input class="border p-2 rounded" v-model="form.frequency" placeholder="Victor Regle" />
        <ul class="text-red-400 text-sm mt-1" v-if="r$.$errors.frequency.length">
          <li v-for="error of r$.$errors.frequency" :key="error">{{ error }}</li>
        </ul>
      </form>
      <Drawer>
        <JSONViewer :data="r$" />
      </Drawer>
    </div>
  </div>
</template>
