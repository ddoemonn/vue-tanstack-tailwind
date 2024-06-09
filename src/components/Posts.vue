<script lang="ts">
import { defineComponent } from 'vue'
import { useQuery } from '@tanstack/vue-query'

import type { Post } from '../types'

const fetcher = async (): Promise<Post[]> =>
  await fetch('https://jsonplaceholder.typicode.com/posts').then((response) =>
    response.json(),
  )

export default defineComponent({
  name: 'PostsList',
  props: {
    isVisited: {
      type: Function,
      required: true,
    },
  },
  emits: ['setPostId'],
  setup() {
    const { isPending, isError, isFetching, data, error, refetch } = useQuery({
      queryKey: ['posts'],
      queryFn: fetcher,
    })

    return { isPending, isError, isFetching, data, error, refetch }
  },
})
</script>

<template>
  <h1 class="text-4xl m-3 p-2 bg-blue-50 inline-flex rounded-xl font-thin">Posts</h1>
  <div v-if="isPending">Loading...</div>
  <div v-else-if="isError">An error has occurred: {{ error }}</div>
  <div v-else-if="data">
    <ul>
      <li v-for="item in data" :key="item.id" class="border-2 border-white bg-blue-50 m-3 font-thin text-xl  rounded-xl p-2 mb-5 ">
        <a
          @click="$emit('setPostId', item.id)"
          href="#"
          class="hover:underline"
          >{{ item.title }}</a
        >
      </li>
    </ul>
  </div>
</template>


