<template>
  <Header />
  <div class="container mx-auto pt-5">
    <Sidebar :datalen="0" />
    <div class="ml-80 flex flex-col justify-center justify-items-center">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.router-link-exact-active {
  color: rgb(17, 152, 107);
}
</style>
