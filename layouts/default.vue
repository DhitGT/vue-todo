<template>
  <Header />
  <div class="container mx-auto pt-5">
    <Sidebar :datalen="0" />
    <div
      class="sm:ml-80 sm:p-10 flex flex-col justify-center justify-items-center"
    >
      <slot />
    </div>
  </div>
  <Footer />
</template>

<style scoped>
.router-link-exact-active {
  color: rgb(17, 152, 107);
}
</style>
