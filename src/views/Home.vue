<script setup lang="ts">
import Drawer from '@/components/Drawer.vue'
import JSONViewer from '@/components/JSONViewer.vue'
import { useRegle } from '@regle/core'
import { required, ipAddress as ipv4Address, integer, between } from '@regle/rules';
import { ref } from 'vue'
import type { Ref } from 'vue'

type Trap = {
    id?: number;
    address: string;
    port: number | null;
};

const form: Ref<Trap> = ref({
    address: '',
    port: 162,
});

const rules = ref({
    address: { ipv4Address, required },
    port: { required, integer, between: between(0, 65_535) }, // TODO: Validate min/max value (1, 65,535)
});

const { r$ } = useRegle(form, rules);
</script>

<template>
  <div class="px-6 text-gray-900 antialiased">
    <div class="mx-auto max-w-xl py-12 md:max-w-4xl">
      <h2 class="text-2xl mb-8">Regle reproduction</h2>
      <div class="flex flex-col">
        <label>Your name</label>
        <input class="border p-2 rounded" v-model="r$.$value.name" placeholder="Victor Regle" />
        <ul class="text-red-400 text-sm mt-1" v-if="r$.$errors.name.length">
          <li v-for="error of r$.$errors.name" :key="error">{{ error }}</li>
        </ul>
      </div>
      <Drawer>
        <JSONViewer :data="r$" />
      </Drawer>
    </div>
  </div>
</template>
