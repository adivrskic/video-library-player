<template>
  <div class="container">
    <Logo />
  </div>
</template>

<script>
import Logo from '../assets/Logo'

export default {
    components: {
        Logo
    }
}
</script>

<style scoped>
    .container {
        background: rgba(0,0,0,.15);
        height: 2.5rem;
        padding: 1rem;
    }

    svg {
        position: relative;
        height: 100%;
    }
</style>
