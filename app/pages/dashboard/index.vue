<template>
  <main>
    <UContainer>
      <section class="pt-24">
        <h1 class="text-3xl jua-font text-white">DashBoard</h1>
      </section>
      <section class="mt-10">
        <LinkForm @created="refresh"/>
      </section>
      <section class="mt-10">        
       <LinkItem 
        v-for="link in data" :key="link.id" 
        :link="{
          shortKey: link.key, 
          longUrl: link.long_url || '', 
          id: link.id,
          totalClicks: link.total_clicks || 0,
          }" 
          class="mb-5"
        />
      </section>
      <section></section>
    </UContainer>
  </main>
</template>

<script setup lang="ts">
import type { Database } from '~~/database.types';

definePageMeta({
    middleware: 'auth'
})
const user = useSupabaseUser()
const client =  useSupabaseClient<Database>()
const { data, refresh } = useAsyncData('links', async () => {
  const { data, error } = await client.from('links').select('*').eq('user_id', user.value?.id)
  return data
})
console.log({data})
</script>

<style scoped>

</style>
