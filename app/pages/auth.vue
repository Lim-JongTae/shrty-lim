<template>
  <div>
    <section class="h-screen grid place-content-center">
      <UContainer>   
        <UCard class="bg-white/5 border border-gray-300/10 rounded-xl shadow-lg min-w-[350px] ">  
          <div class="text-center" >
            <div class="flex justify-center" >
              <div class="flex justify-center w-20 h-20 items-center border border-white/30 rounded-full shadow-xl">
                <svg xmlns="http://www.w3.org/2000/svg" 
                  width="32" 
                  height="32" 
                  viewBox="0 0 24 24">
                  <path fill="none" 
                  stroke="#888888" 
                  stroke-linecap="round" 
                  stroke-linejoin="round" 
                  stroke-width="1.5" 
                  d="M16.5 10.5V6.75a4.5 4.5 0 1 0-9 0v3.75m-.75 11.25h10.5a2.25 2.25 0 0 0 2.25-2.25v-6.75a2.25 2.25 0 0 0-2.25-2.25H6.75a2.25 2.25 0 0 0-2.25 2.25v6.75a2.25 2.25 0 0 0 2.25 2.25"
                  />
                </svg>        
              </div>
            </div>  
            <UButton color="orange" class="py-4 px-3 rounded-full mt-3" block @click="handleGithublogin">
              Continue with Github
            </UButton>        
            
          </div>                          
        <form class="mt-5" @submit.prevent="handleLoginForm">
          <div class="mb-5">
            <label for="email">Email</label>
            <input v-model="form.email" type="email" name="email" id="email" placeholder="test@naver.com" />
          </div>
          <div>
            <label for="password">Password</label>
            <input v-model="form.password" type="password" name="password" id="password" placeholder="6자리 이상" />
          </div>
          <UButton type="submit" color="orange" block class="rounded-full py-3 mt-5 jua-font text-xl mb-3">
            <template v-if="isLoggingIn">로그 인</template>
            <template v-else>계정 만들기</template>
          </UButton>          
          <div class="text-center">
            <button type="button" @click="isLoggingIn=!isLoggingIn">
              <template v-if="isLoggingIn"> 아직 계정이 없습니까? 계정만들기</template>
              <template v-else>이미 계정이 있읍니까? 로그 인</template>             
            </button>
          </div>
          <div class="text-red-600 text-center">{{ errors }}</div>
        </form>
      </UCard>
      </UContainer>
    </section>
  </div>
</template>

<script setup lang="ts">
const errors = ref<string>('')
const supabaseAuth = useSupabaseClient()
const isLoggingIn = ref<boolean>(true)
const form = ref<{
  type: 'login' | 'register';
  email: string;
  password: string;
}>({
  type: 'login',
  email: '',
  password: '',
})
const isLoading = ref(false)
const handleGithublogin = async () => {  
  const { auth } = useSupabaseClient()
  try {
    isLoading.value = true
    auth.signInWithOAuth({
      provider: 'github',
      options: {
        redirectTo: 'http://localhost:3000'
      }
    })
  } catch (error) {
    console.log(error)
  } finally {
    isLoading.value = false
  }
  }

const handleLoginForm = async () => {
  
  if ( !form.value?.email || !form.value?.password) {
    errors.value = '입력값이 없습니다.'
    return
  }
  if (!isLoggingIn.value) {
    return handleSignup()
  }
  try { 
    const { data, error } = await supabaseAuth.auth.signInWithPassword({
      email: form.value?.email,
      password: form.value?.password     
    })
    if (error) {
      errors.value = error.message
      return
    }
    if (data) {
      useRouter().push('/dashboard')
      console.log({data})
      errors.value = ''
    }
  } catch (err) {
    errors.value = 'Not logging'
  }
  
}
const handleSignup = async () => {
  try {
    const { data, error } = await supabaseAuth.auth.signUp({
      email: form.value?.email,
      password: form.value?.password
    })
    if (error) {
      errors.value = error.message
      return
    }
    console.log({ data })
  } catch (error) {
    console.log(error)
  }
}


</script>

<style scoped>

</style>