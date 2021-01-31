<template>
  <div>
      <nav>
        <div class="nav-wrapper">
          <a href="#" class='my_nav'>Enye Customer Records</a>
        </div>
      </nav>
    <Records />
  </div>
</template>

<script>
import Records from './components/Records'

export default {
  components: { Records },
  name: 'app'
}
</script>

<style scoped>
  .my_nav{
    color: aliceblue;
    font-size: 19px;
    font-family: Arial, Helvetica, sans-serif;
    margin-left: 50px;

  }
</style>