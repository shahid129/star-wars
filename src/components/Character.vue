<template>
  <div class="container-fluid">
    <div class="row justify-content-center">
      <!-- search bar -->
      <input
        class="my-4 px-5 col-10"
        type="text"
        v-model="search"
        placeholder="Search Heroes"
        @change="filterdCharacter"
      />
      <!-- Add Pagination -->
      <div
        v-for="char in currentPagePosts"
        :key="char.id"
        class="char col-sm-6 col-md-4 col-lg-3 d-flex justify-content-center"
      >
        <!-- <router-link :to="{name: 'name', params: {id: char.id}}"> -->
        <div class="card m-1 mb-3" style="width: 14rem">
          <img
            class="img-responsive card-img-top"
            src="https://images.unsplash.com/photo-1483134529005-4c93495107d5?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1171&q=80"
            alt="Image of"
          />
          <div class="card-body">
            <h5 class="card-title">{{ char.name }}</h5>
            <p class="card-text">W: {{ char.mass }}</p>
            <p class="card-text">H: {{ char.height }}</p>
            <a @click="toggleModal" href="#" class="btn btn-primary"
              >Home World</a
            >
          </div>
        </div>
        <!-- </router-link> -->
      </div>
    </div>
  </div>
  <!-- <div v-for="x in formatcharacter">
    <p>{{x}}</p>

  </div> -->
  <!-- Dialog start-->
  <!-- <button @click="toggleModal">Open Modal</button> -->
  <div v-if="showModal">
    <div class="backdrop" @click.self="closeModal">
      <div class="dialog">
        <!-- <h1>{{ title }}</h1> -->
        <!-- <div v-for="x in formatcharacter" :key="x.id">
          <p>{{ x.name }}</p>
        </div> -->
        <!-- <p>{{ text }}</p>
        <p>{{ header }}</p> -->
        <button @click="toggleModal" type="button" class="btn btn-primary">
          Close
        </button>
      </div>
    </div>
  </div>

  <!-- End Dialog -->

  <!-- button for pervious and nex page -->
  <div>
    <button class="btn btn-primary m-3" @click="setCurrentPage(-1)">
      PREV
    </button>
    <button class="btn btn-primary m-3" @click="setCurrentPage(1)">NEXT</button>
    <p>
      Page: {{ currentPage }} / {{ Math.ceil(character.length / postsPerPage) }}
    </p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentPage: 1,
      postsPerPage: 10, // Number of post per page
      character: [],
      id: {
        id: "",
      },
      search: "", // search bar
      title: "lets play the game",
      header: "Time to get a job",
      text: "Lets do it",
      showModal: false,
    };
  },

  //   Get info from the API
  mounted() {
    const getPeople = async () => {
      let nextPage = "https://swapi.dev/api/people/";

      let people = [];

      while (nextPage) {
        const res = await fetch(nextPage);
        const { next, results } = await res.json();
        nextPage = next;
        people = [...people, ...results];

        this.character = people;
      }

      console.log(people.length);
      console.log(people);
    };
    getPeople();
  },

  //   Dialog
  methods: {
    closeModal() {
      this.$emit("close");
    },
  },

  methods: {
    toggleModal() {
      this.showModal = !this.showModal;
    },
    setCurrentPage(direction) {
      if (direction === -1 && this.currentPage > 1) {
        this.currentPage -= 1;
      } else if (
        direction === 1 &&
        this.currentPage < this.character.length / this.postsPerPage
      ) {
        this.currentPage += 1;
      }
    },
  },

  //   Search bar
  computed: {
    // filterdCharacter() {
    //   return this.character.filter((blog) => {
    //     return blog.name.toLowerCase().includes(this.search.toLowerCase());
    //   })
    // },

    currentPagePosts() {
      // add a variable to query search character
      let searchChar = this.character.filter((blog) => {
        return blog.name.toLowerCase().includes(this.search.toLowerCase());
      })

      // add a variable to query pagination
      let paginate = this.character.slice(
        (this.currentPage - 1) * this.postsPerPage,
        this.currentPage * this.postsPerPage)
        // console.log(y, x)

        let search = this.search
        if (search) {
          return searchChar
        } else {
          return paginate
        } 
    },

    formatcharacter() {
      // return this.character.map((person) => {
      //   let homeworld = person.homeworld;
      //   console.log(homeworld);

      //   planets = []

      //   fetch("homeworld")
      //       .then((response) => response.json())
      //       .then((data) => (this.planets = data.results))
      //       console.log(planets)
      //   //     .then((data) => console.log(data))
      //   // return homeworld;

      //   // return `${person.name} is ${person.eye_color}`
      // });
      
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

input {
  text-align: center;
}

/* Put plcaeholder in the center */
::placeholder {
  text-align: center;
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