<template>
  <header class="border-white/20 shadow-lg fixed top-0 left-0 right-0 backdrop-blur-md z-50">
    <nav class="container mx-auto items-center py-2 flex justify-between  ">
      <NuxtLink to="/" class="navar-brand text-2xl flex items-center">
        <NuxtImg src="/icon-green.png" format="webp" width="56px" />
        <span class="jua-font font-bold text-2xl">Shrty3</span>
      </NuxtLink>
      <ul class="font-bold flex justify-center items-center gap-16">
        <li v-for="link in navLinks" :key="link.to" class="text-xl">
          <NuxtLink :to="link.to" class="-mr-10">{{ link.label }}</NuxtLink>
        </li>     
        <li v-if="!user">
          <NuxtLink to="/auth"><UButton color="orange" class="px-8 py-2 rounded-full">Sign In</UButton></NuxtLink>
        </li>   
        <li v-else>
          <UButton @click="handleLogout" color="orange">Logout</UButton>
        </li>
      </ul>
    </nav>
  </header>
</template>

<script lang="ts" setup>
const user = useSupabaseUser()
const auth =  useSupabaseClient()
const navLinks = ref<
  {
    to: string,
    label: string,
  }[]
>([
  {
  to: '/',
  label: 'Home'
  },
  {
    to: '/about',
    label: 'About'
  },
  {
    to: '/content',
    label: 'Content'
  }
])
const handleLogout = async () => {
  await auth.auth.signOut()
  useRouter().push('/')
}
</script>

<style scoped>

</style>