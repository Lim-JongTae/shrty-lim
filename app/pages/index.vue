<template>
  <div>
    <section class="h-screen grid place-content-center">
      <div class="max-w-3xl mx-auto text-center">
        <h1 class="text-5xl font-bold heading">
          Make your links tiny and easy to share with tnyl
        </h1>
        <p class="my-10 text-lg w-10/12 mx-auto md:w-2/3">
          Transform your long, combersome URLs into short and shareable links with TinyLink
        </p>
        <NuxtLink to="/dashboard">
          <UButton color="orange" size="lg" class="px-8 py-4 rounded-full">Short Now!</UButton>
        </NuxtLink>
      </div>
    </section>
  </div>
</template>

<script setup>

</script>

<style scoped>
.heading {
  background: linear-gradient(180deg, #fff 0%, rgba(255,255,255,0) 189%);
  background-clip: text;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
</style>