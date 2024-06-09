<script lang="ts">
import { defineComponent } from 'vue'
import { useQuery } from '@tanstack/vue-query'

import { Post } from '../types'

const fetcher = async (id: number): Promise<Post> =>
  await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`).then(
    (response) => response.json(),
  )

export default defineComponent({
  name: 'PostDetails',
  props: {
    postId: {
      type: Number,
      required: true,
    },
  },
  emits: ['setPostId'],
  setup(props) {
    const { isPending, isError, isFetching, data, error } = useQuery({
      queryKey: ['post', props.postId],
      queryFn: () => fetcher(props.postId),
    })

    return { isPending, isError, isFetching, data, error }
  },
})
</script>

<template>
  <h1 class="p-2 m-3 mb-2 mt-5 rounded-xl bg-blue-50 inline-flex">Post {{ postId }}</h1>
 
  <div v-if="isPending" class="bg-blue-500">Loading...</div>
  <div v-else-if="isError" class="bg-red-500">An error has occurred: {{ error }}</div>
  <div v-else-if="data" class="border-2 border-white bg-blue-50 m-3   rounded-xl p-2 mb-5 font-thin text-xl">
    <h1>{{ data.title }}</h1>
    <div>
      <p>{{ data.body }}</p>
    </div>
    <div v-if="isFetching" class="update">Background Updating...</div>
  </div>
  <a @click="$emit('setPostId', -1)" href="#" class="border-2 border-white bg-blue-50 m-3 mt-10  hover:text-blue-400 hover:border-blue-400 rounded-xl p-2"> Back </a>
</template>

