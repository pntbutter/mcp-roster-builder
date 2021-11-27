<template>
  <div class="min-h-screen  md:p-4  md:pt-14  bg-gray-900  text-white  text-opacity-90">
    <Html >
      <Head>
        <Title>MCP Roster builder</Title>
      </Head>
    </Html>
    
    <div class="container  mx-auto">
      <NuxtPage /> 
    </div>

    <nav class="fixed  inset-x-0  bottom-0  md:bottom-auto  md:top-0  h-12  flex  items-center  justify-center  space-x-4">
      <NuxtLink to="/">Characters</NuxtLink>
      <NuxtLink to="/tactics-cards">Tactics</NuxtLink>
      <NuxtLink to="/crisis-cards">Crisis</NuxtLink>
      <NuxtLink to="/roster">Roster</NuxtLink>
    </nav>

  </div>
</template>

<style scoped>
  .container {
    max-width: 768px;
  }
</style>