<template>
  <div class="contaner-fluid">
    <div class="row justify-content-center">
      <div
        v-for="char in character.results"
        :key="char.id"
        class="char col-sm-6 col-md-4 col-lg-3">
        <!-- <router-link :to="{name: 'name', params: {id: char.id}}"> -->
        <div class="card m-1 mb-3" style="width: 14rem">
          <img
            class="img-responsive card-img-top"
            src="https://images.unsplash.com/photo-1483134529005-4c93495107d5?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1171&q=80"
            alt="Image of"/>
          <div class="card-body">
            <h5 class="card-title">{{ char.name }}</h5>
            <p class="card-text">W: {{ char.mass }}</p>
            <p class="card-text">H: {{ char.height }}</p>
            <a @click="toggleModal" href="#" class="btn btn-primary"
              >Home World</a>
          </div>
        </div>
        <!-- </router-link> -->
      </div>
    </div>
  </div>
  <!-- Dialog start-->
  <button @click="toggleModal">Open Modal</button>
  <div v-if="showModal">
    <div class="backdrop" @click.self="closeModal">
      <div class="dialog">
        <!-- <h1>{{ title }}</h1> -->
        <p>{{ text }}</p>
        <p>{{ header }}</p>
        <button @click="toggleModal" type="button" class="btn btn-primary">
          Close
        </button>
      </div>
    </div>
  </div>

  <!-- End Dialog -->
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
      title: "lets play the game",
      header: "Time to get a job",
      text: "Lets do it",
      showModal: false,
    };
  },

//   Get info from the API
  mounted() {
    fetch("https://swapi.dev/api/people/")
      .then((response) => response.json())
      .then((data) => (this.character = data))
      .catch((err) => console.log(err.message));
  },

  //   Dialog
  methods: {
    closeModal() {
      this.$emit("close");
    }
  },

  methods: {
    toggleModal() {
      this.showModal = !this.showModal;
    },
  },
};
</script>

<style>
img {
  border-radius: 60%;
  width: 50%;
  margin: auto;
}

/* Dialog */
.dialog {
  position: relative;
  width: 40%;
  padding: 20px;
  margin: 100px auto;
  background: white;
  border-radius: 10px;
}

.backdrop {
  top: 0;
  position: fixed;
  background: rgba(0, 0, 0, 0.5);
  width: 100%;
  height: 100%;
}

/* Media Query */
@media only screen and (max-width: 792px) {
  .dialog {
    width: 70%;
  }
}

@media only screen and (max-width: 600px) {
  .dialog {
    width: 90%;
  }
}
</style>