<template>
  <div class="contaner-fluid">
    <div class="row justify-content-center  ">
        <div v-for="char in character.results" :key="char.id" class="char col-sm-6 col-md-4 col-lg-3 ">
            <router-link :to="{name: 'name', params: {id: char.id}}">
                <div class="card m-1 mb-3 " style="width: 14rem">
                    <img class="img-responsive card-img-top" src="https://images.unsplash.com/photo-1483134529005-4c93495107d5?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1171&q=80" alt="Image of" />
                    <div class="card-body">
                        <h5 class="card-title">{{char.name}}</h5>
                        <p class="card-text">W: {{ char.mass}}</p>
                        <p class="card-text">H: {{ char.height}}</p>
                        <a href="#" class="btn btn-primary">Go somewhere</a>
                    </div>
                </div>
            </router-link>
        </div>
      </div>
    </div>
</template>
<!-- <div v-for="char in character.results" :key="char.id" class="char col-sm-3">
    <router-link :to="{name: 'name', params: {id: char.id}}">
        <h2>{{char.name}}</h2>
        <p>{{ char.mass}}</p>
        <button v-on:click="homeWorld" class="btn btn-primary">Home World</button>
    </router-link>
</div> -->

<script>
export default {
  data() {
    return {
      character: [],
    };
  },
  mounted() {
    fetch("https://swapi.dev/api/people/")
      .then((response) => response.json())
      .then((data) => (this.character = data))
      .catch((err) => console.log(err.message));
  },
};
</script>

<style>
img {
    border-radius: 60%;
    width: 50%;
    margin: auto;

}

</style>