<template>
  <div class="home">
    <SiteHeader/>
    <div id="carouselExampleCaptions" class="carousel slide" data-bs-ride="false">
      <div class="carousel-indicators">
        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="0" class="active" aria-current="true" aria-label="Slide 1"></button>
        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="1" aria-label="Slide 2"></button>
        <button type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide-to="2" aria-label="Slide 3"></button>
      </div>
      <div class="carousel-inner">
        <div class="carousel-item active">
          <img src="../assets/menu/plantes-innovation-getty.jpg" class="d-block w-100" alt="...">
          <div class="carousel-caption d-none d-md-block">
            <button @click="startGame('plants')" class="btn btn-primary btn-lg"><h5>Plants</h5></button>
            <p>Some representative placeholder content for the first slide.</p>
          </div>
        </div>
        <div class="carousel-item">
          <img src="../assets/menu/coccinelle2-jpg-1400x950.jpg" class="d-block w-100" alt="...">
          <div class="carousel-caption d-none d-md-block">
            <button @click="startGame('insects')" class="btn btn-primary btn-lg"><h5>Insects</h5></button>
            <p>Some representative placeholder content for the second slide.</p>
          </div>
        </div>
        <div class="carousel-item">
          <img src="../assets/menu/2c1074k-e1638197792976.jpg" class="d-block w-100" alt="...">
          <div class="carousel-caption d-none d-md-block">
            <button @click="startGame('animals')" class="btn btn-primary btn-lg"><h5>Animals</h5></button>
            <p>Some representative placeholder content for the third slide.</p>
          </div>
        </div>
      </div>
      <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="prev">
        <span class="carousel-control-prev-icon" aria-hidden="true"></span>
        <span class="visually-hidden">Previous</span>
      </button>
      <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleCaptions" data-bs-slide="next">
        <span class="carousel-control-next-icon" aria-hidden="true"></span>
        <span class="visually-hidden">Next</span>
      </button>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import SiteHeader from '@/components/SiteHeader.vue';

export default {
  name: 'HomeView',
  components: {
    SiteHeader
  },
  methods:{
    startGame(type){
      this.$router.push('/game/'+type);
    }
  }
}
</script>

<style lang="scss" scoped>
  .home{
    background-color: #4C5C68;
    height: 100vh;
  }

  .carousel-inner{
    img{
      height: 90vh;
      width: auto;
    }
  }

  .carousel-caption{
    button{
      height: 10vh;
      width: 10vw;
      position: relative;
      top: -10vh;
      background-color: #46494C;
      border-color: #1985A1;
    }
  }

</style>
