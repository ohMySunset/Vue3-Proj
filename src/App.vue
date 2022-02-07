<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
  <router-link class="navbar-brand" to="/">Vue App</router-link>

    <ul class="navbar-nav mr-auto">
      <li class="nav-item active">
        <router-link class="nav-link" to="/todos">Todo</router-link>
      </li>
    </ul>
</nav>
  <router-view/>
</template>

<script>
export default {

}
</script>

<style>

</style>
   