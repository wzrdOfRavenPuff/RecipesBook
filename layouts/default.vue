<template>
  <div>
    <header>
      <nav class="container">
        <NuxtLink to="/" class="font-bold">Home</NuxtLink>
        <ul class="">
          <li><NuxtLink to="/todo">Todo</NuxtLink></li>
          <li><NuxtLink to="/about">About</NuxtLink></li>
        </ul>
        <CommonDarkModeIcon />
      </nav>
    </header>
    <!-- output the page content -->
    <div class="container mx-auto p-4">
      <slot />
    </div>
    <!-- <footer class="container mx-auto p-4 flex justify-between border-t-2">
    </footer> -->
  </div>
</template>
<script setup></script>
<style scoped></style>
