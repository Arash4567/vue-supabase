<script setup>
import { ref, onMounted } from 'vue'
import { supabase } from '@/supabaseClient'

const name = ref('')
const countries = ref([])

async function getCountries() {
  const { data } = await supabase
  .from('countries')
  .select()
  countries.value = data
}

onMounted(() => {
  getCountries()
})

const addName = async () => {
  let { error } = await supabase.from('countries').insert({id: countries.value[countries.value?.length - 1]?.id + 1, name: name.value})
  if (error) {
    throw error
  } else {
    getCountries()
    name.value = ''  
  }
}
</script>
<template>
  <h1>This is an about page</h1>
  <div>
    <h2>Enter Country Name: </h2>
    <input type="text" v-model="name">
    <button @click="addName()">Save</button>
    <ul>
      <li v-for="country in countries" :key="country.id">{{ country?.name }}</li>
    </ul>
  </div>
</template>

<style></style>
