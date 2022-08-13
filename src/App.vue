<script>
import spring from './pages/spring.vue'
import train from './pages/train.vue'
import about from './pages/about.vue'

const routes = {
  '/': spring,
  'train': train,
  'about': about
}

export default {
  data() {
    return {
      currentPath: window.location.pathname
    }
  },
  computed: {
    currentView() {
      return routes[this.currentPath.slice(1) || '/'] || NotFound
    }
  },
  mounted() {
    window.addEventListener('hashchange', () => {
      this.currentPath = window.location.pathname
    })
  }
}
</script>

<template>
  <div class="min-h-full h-full">
    <nav class="flex items-center justify-between max-w-3xl p-4 mx-auto">
      <a class="inline-flex items-center justify-center w-8 bg-gray-100 rounded-lg" href="/">
        <img src="./src/logo.png">
      </a>

      <ul class="flex items-center space-x-2 text-sm font-medium text-gray-500">
        <li class="hidden lg:block">
          <a class="px-3 py-2 rounded-lg" href="/"> Spring </a>
        </li>

        <li><a class="px-3 py-2 rounded-lg" href="train"> 训练计划 </a></li>

        <li>
          <a class="inline-flex items-center px-3 py-2 rounded-lg" href="about">
            关于
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor"
              class="ml-1.5 w-4 h-4">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14"></path>
            </svg>
          </a>
        </li>
      </ul>
    </nav>
    <div class="p-4 max-w-3xl mx-auto">
      <component :is="currentView" />
    </div>
  </div>
</template>