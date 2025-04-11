<template>
    <div class="flex flex-col min-h-screen">
      <nav class="bg-teal-500 dark:bg-teal-700 py-6 px-8">
        <div class="container mx-auto max-w-8xl flex justify-between max-sm:hidden">
          <ul class="flex items-center space-x-5 text-white">
            <li>
              <NuxtLink to="/" class="flex items-center space-x-2">
                <UIcon name="i-lucide-building-2" class="size-8" />
                <label class="text-xl">BIMprove</label>
              </NuxtLink>
            </li>
            <li>
              <NuxtLink to="/" class="text-lg relative px-1">
                Home
                <span
                  class="absolute left-0 -bottom-1 h-0.5 bg-white rounded-full transition-all duration-300 ease-in-out"
                  :class="{ 'w-full': $route.path === '/', 'w-0': $route.path !== '/' }"
                ></span>
              </NuxtLink>
            </li>
            <li>
              <NuxtLink to="/view3d" class="text-lg relative px-1">
                3D View
                <span
                  class="absolute left-0 -bottom-1 h-0.5 bg-white rounded-full transition-all duration-300 ease-in-out"
                  :class="{ 'w-full': $route.path === '/view3d', 'w-0': $route.path !== '/view3d' }"
                ></span>
              </NuxtLink>
            </li>
          </ul>
          <ColorModeSelector />
        </div>
        
        <div class="sm:hidden">
          <UIcon name="i-lucide-menu" class="size-8 bg-gray-50" @click="toggleMenu" />
        </div>

        <div v-show="isMenuOpen" class="sm:hidden bg-teal-500 dark:bg-teal-700 py-4">
          <ul class="flex flex-col space-y-4 text-white">
            <li class="flex items-center space-x-2">
                <UIcon name="i-lucide-building-2" class="size-8" />
                <label class="text-xl">BIMprove</label>
            </li>
            <li>
              <NuxtLink to="/" class="text-lg">Home</NuxtLink>
            </li>
            <li>
              <NuxtLink to="/view3d" class="text-lg">3D View</NuxtLink>
              
            </li>
            <li>
                <ColorModeSelector />
            </li>
          </ul>
        </div>
      </nav>

      <main class="flex-1 container mx-auto max-w-8xl">
        <slot></slot>
      </main>
  
      <footer class="bg-teal-700 dark:bg-teal-500 text-white py-10 mt-10 px-8">
        <div class="text-center space-y-4 container mx-auto max-w-8xl">
          <div class="grid grid-cols-1 sm:grid-cols-2 gap-8">
            <div>
              <h1 class="my-5 text-2xl">SOCIAL MEDIA</h1>
              <div class="flex justify-center space-x-6">
                <a href="https://facebook.com" target="_blank" rel="noopener" class="hover:text-gray-300">
                  <UIcon name="i-lucide-facebook" class="size-8" />
                </a>
                <a href="https://twitter.com" target="_blank" rel="noopener" class="hover:text-gray-300">
                  <UIcon name="i-lucide-twitter" class="size-8" />
                </a>
                <a href="https://linkedin.com" target="_blank" rel="noopener" class="hover:text-gray-300">
                  <UIcon name="i-lucide-linkedin" class="size-8" />
                </a>
              </div>
            </div>
            <div>
              <h1 class="my-5 text-2xl">CONTACT US</h1>
              <p>example@email.com</p>
              <p>+123 456 7890</p>
            </div>
          </div>
          <p class="mt-8 text-center">&copy; 2023 BIMprove. All rights reserved.</p>
        </div>
      </footer>
    </div>
  </template>
  
  <script setup lang="ts">
    const isMenuOpen = ref(false)

    const toggleMenu = () => {
      isMenuOpen.value = !isMenuOpen.value
    }
  </script>
  
