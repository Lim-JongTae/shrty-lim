<template>
  <div class="container">
  <div v-if="data" class="pt-20">
    {{ data }}
    <h1 class="text-5xl text-amber-500 font-bold">{{ data.key }}</h1>
    <div class="mt-5">{{ data.created_at }}</div>
    <div class="mt-5 flex gap-5">
      <div class="card">
        <div class="text-2xl text-white/40 overflow-x-auto" texteditable>{{ data.long_url }}</div>
      </div>
      <div class="card">
        <div class="text-2xl text-white/40">{{ data.total_clicks }}</div>
        <div>Total Clicks</div>
      </div>
  </div>
  <div class="container mt-10">
        <h2 class="text-2xl mb-5">clicks Logs</h2>
        <div v-for="click in data.clicks" class="card">{{ click }}</div>
      </div>
  </div>
</div>
</template>

<script setup lang="ts">
import type { Database } from '~~/database.types';

const { id } = useRoute().params
const { data } = useAsyncData(id.toString(), async () => {
    const client = useSupabaseClient<Database>()
    const { data:res, error } = await client
      .from('links')
      .select('*, clicks(*)')
      .eq('key', id)
      .single()
    return res
  }
);
</script>

<style scoped>

</style>