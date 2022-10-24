<template>
  <div class="home">
    <h4>home</h4>
  </div>

  <div id="navbarSupportedContent">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li>
            <router-link to="/activos">Activos</router-link>
        </li>
        <li>
            <router-link to="/areas">Areas</router-link>
        </li>
        
      </ul>

    </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'HomeView',
  components: {
  }
}
</script>
