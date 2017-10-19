<template>
  <div>
    <header>
      <div class="branding">     
      </div>
      <nav>
        <a href="#/">Home</a>
        <a href="#/vueLineChart">Grafico Valoracion</a>
        <a href="#/actors">Actors</a>
        <a href="#/newActor">New actors</a>
      </nav>
    </header>

    <main class="container">
      <router-view></router-view>
    </main>
    <footer>
      Footer
    </footer>
</div>
</template>

<script>
export default {

}
</script>
