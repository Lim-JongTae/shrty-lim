<template>
  <div>
    {{ id }}
    {{ data }}
  </div>
</template>

<script setup lang="ts">
import useExtranalRedirect from '~/composables/useExtranalRedirect';
import type { Database } from '~~/database.types';
import geoip from 'geoip-lite';

const route = useRoute();
const id = route.params.id
const client = useSupabaseClient<Database>()
if (!id) {
  throw createError({
    statusCode: 404,
    statusMessage: 'Not Found'
  })
}
const { data } = await useAsyncData('link', async () => {
  const { data, error } = await client
    .from('links')
    .select('*')
    .eq('key', id)
    .single()

  if (error) {
    throw createError({
      statusCode: 404,
      statusMessage: 'Not found'
    })
  }
  return data
}) 
if (data.value?.long_url) {
  const ua = useUserAgent()
  console.log(ua)

  if (ua && ua.ip) {
    const geoLocation = geoip.lookup(ua.ip)
    const { data:res, error} = await client.from('clicks').insert({
      link_id: data.value.id,
      ip: ua.ip,
      country: geoLocation?.country,      
      city: geoLocation?.city,
      user_agent: ua.userAgent
    })
    }

  useExtranalRedirect(data.value?.long_url)
}
</script>

<style scoped>

</style>