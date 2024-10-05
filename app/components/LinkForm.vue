<template>
  <div>
    <div class="card">
      <form @submit.prevent="handleLinkForm">
        <div class="flex gap-5 items-end">
          <div class="w-6/12">
            <label for="url" class="text-white">Long URL</label>
            <input v-model="form.long_url" type="text" id="url" class="input">
          </div>
          <div class="w-4/12">
            <label for="shortKey" class="text-white">Short Key</label>
            <input v-model="form.key" type="text" id="shortKey" class="tracking-widest text-center">
          </div>             
          <div class="w-2/12">
            <UButton type="submit" block color="orange" class="mb-2 py-2 text-md rounded-lg">Short</UButton>
          </div>             
        </div>
      </form>
    </div>
  </div>
</template>

<script setup lang="ts">
import { nanoid } from 'nanoid';
import type { Database } from '~~/database.types';

const client = useSupabaseClient<Database>()
const user = useSupabaseUser()

const form = reactive({
  long_url: '',
  key: '',
  
})
const emit = defineEmits(['created'])

const handleLinkForm = async () => {
  try {
    const { data, error } = await client.from('links').insert({
      long_url: form.long_url,
      key: form.key,
      user_id: user.value?.id,
    })
    if (error) {
      alert(error.message)
      return
    }
    
      createShortKey()
      form.long_url = ''
      alert('Link created successfully')
      emit('created',1)
    
  } catch (error) {
    console.log(error)
  }
}
const createShortKey = (len: number = 6): void => {
  form.key = nanoid(len)
}
onMounted(() => {
  createShortKey()
})
</script>

<style scoped>

</style>