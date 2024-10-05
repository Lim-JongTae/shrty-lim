<template>
   <div class="card">
      <div class="flex justify-between">
        <div class="link-info">
          <NuxtLink 
            :to="`/dashboard/${link.shortKey}`" 
            class="text-amber-500 text-2xl roboto-font font-bold"
            >/{{ link.shortKey }}
          </NuxtLink>
          <div class="text-xl text-white/50 roboto-font">{{ link.longUrl.slice(0, 30) + '...' }}</div>
        </div>
        <div class="flex gap-2 items-center">
          <div class="text-2xl">
            <UIcon name="i-heroicons:arrow-trending-up" class="text-green-500" />
            {{ link.totalClicks }}
          </div>
        </div>
        <UButton color="orange" class="w-12 h-12 grid place-content-center rounded-full">
          <UIcon name = "i-heroicons:clipboard-document" class="w-7 h-7" @click="handleCopy" />            
        </UButton>
      </div>
    </div>
</template>

<script setup lang="ts">
const props = defineProps<{
  link: {
    shortKey: string;
    longUrl: string;
    id: number;
    totalClicks: number
  }
}>()
const config = useRuntimeConfig()

const handleCopy = () => {
  navigator.clipboard.writeText(`${config.public.appUrl}/${props.link.shortKey}`)
  alert('Copied ti clipboard')
}
</script>

<style scoped>

</style>