<template>
  <h1>Dashboard</h1>
  <p v-if="loading">loading...</p>
  <pre v-else>User: {{ user }}</pre>
  <a href="" @click.prevent="authLogout()">Logout</a>
</template>

<script setup lang="ts">
import type { ObjectId } from "mongodb"

interface User {
  _id: ObjectId
  userId: string
  email: string
  password: string
  timezone: string
}

definePageMeta({ middleware: "auth" })

const user = ref<User | null>(null)
const loading = ref(true)

async function getUser() {
  loading.value = true
  try {
    user.value = await authUser()
  } catch (error) {
    console.log("Error getting user")
  }
  loading.value = false
}

onMounted(() => getUser())
</script>
