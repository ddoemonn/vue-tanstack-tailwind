<script lang="ts">
import { defineComponent, ref } from 'vue'
import { VueQueryDevtools } from '@tanstack/vue-query-devtools'

import Posts from './components/Posts.vue'
import Post from './components/Post.vue'

export default defineComponent({
  name: 'App',
  components: { Posts, Post, VueQueryDevtools },
  setup() {
    const visitedPosts = ref(new Set())
    const isVisited = (id: number) => visitedPosts.value.has(id)

    const postId = ref(-1)
    const setPostId = (id: number) => {
      visitedPosts.value.add(id)
      postId.value = id
    }

    return {
      isVisited,
      postId,
      setPostId,
    }
  },
})
</script>

<template>
  <h2 class="bg-blue-700 text-white">Vue 3 + TanStack Query</h2>
  <Post v-if="postId > -1" :postId="postId" @setPostId="setPostId" />
  <Posts v-else :isVisited="isVisited" @setPostId="setPostId" />
  <VueQueryDevtools />
</template>
