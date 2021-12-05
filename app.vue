<template>
  <div class="bg-gray-900  text-white  text-opacity-90">
    <Html >
      <Head>
        <Title>MCP Battles</Title>
      </Head>
    </Html>

    <nav class="md:bottom-auto  md:top-0  h-12  flex  items-center  justify-center  bg-black  bg-opacity-20  text-sm  uppercase  font-bold  tracking-widest  antialiased">
      <NuxtLink class="py-2  px-3  text-gray-400  hover:text-gray-200" to="/">Battles</NuxtLink>
      <NuxtLink class="py-2  px-3  text-gray-400  hover:text-gray-200" to="/roster">Roster</NuxtLink>
      <NuxtLink class="py-2  px-3  text-gray-400  hover:text-gray-200" to="/play">Play</NuxtLink>
    </nav>
    
    <NuxtPage class="p-4" /> 

  </div>
</template>

<style>
body {
  @apply bg-gray-900;
}
.router-link-active {
  color: #fff;
}
</style>