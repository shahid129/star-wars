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
        v-for="(char, index) in currentPagePosts"
        :key="char.id"
        class="char col-sm-6 col-md-4 col-lg-3 d-flex justify-content-center"
      >
        <div class="card m-1 mb-3" style="width: 14rem">
          <img
            class="img-responsive card-img-top"
            src="https://images.unsplash.com/photo-1599719500956-d158a26ab3ee?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1171&q=80"
            alt="Image of"
          />
          <div class="card-body">
            <h4 class="card-title">{{ char.name }}</h4>
            <p class="card-text">W: {{ char.mass }}</p>
            <p class="card-text">H: {{ char.height }}</p>
            <a @click="toggleModal(index)" href="#" class="btn btn-primary"
              >Home World</a
            >
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- Dialog start-->
  <div v-if="showModal">
    <div class="backdrop overflow-auto" @click.self="closeModal">
      <div class="dialog container-fluid">
        <div v-if="home">
          <!-- modal Image -->
          <img class="card-img-top home-image" src="https://images.unsplash.com/photo-1487715433499-93acdc0bd7c3?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1844&q=80" alt="Card image cap">

          <div class="d-flex mx-auto w-auto" style="width: 18rem">
            <div class="card-body container-fluid">
              <h2 class="card-title text-white home-name">{{ home.name }}</h2>
              <div class="row container overflow-auto">
                
                <div class="col-md-6 col-sm-12">
                  <!-- Nested rows and cols. switch to row as screen gets smaller -->
                  <div class="row">
                    <div class="col-md-12 col-sm-6">
                      <p class="fw-bold">Climate</p>
                    </div>
                    <div class="col-md-12 col-sm-6">
                      <p>{{ home.climate }}</p>
                    </div>
                    <div class="col-md-12 col-sm-6">
                      <p class="fw-bold">Population</p>
                    </div>
                    <div class="col-md-12 col-sm-6">
                      <p>{{ home.population }}</p>
                    </div>
                  </div>
                </div>
                <div class="col-md-6 col-sm-12">
                  <div class="row">
                    <div class="col-md-12 col-sm-6">
                      <p class="fw-bold">Terrain</p>
                    </div>
                    <div class="col-md-12 col-sm-6">
                      <p>{{ home.terrain }}</p>
                    </div>
                    <div class="col-md-12 col-sm-6">
                      <p class="fw-bold">Gravity</p>
                    </div>
                    <div class="col-md-12 col-sm-6">
                      <p>{{ home.gravity }}</p>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        <button @click="closeModal" type="button" class="btn btn-primary">
          Close
        </button>
      </div>
    </div>
  </div>
  <!-- End Dialog -->

  <!-- button for pervious and nex page -->
  <div>
    <button class="btn btn-primary btn-pagination m-3" @click="setCurrentPage(-1)">
      PREV
    </button>
    <button class="btn btn-primary btn-pagination  m-3" @click="setCurrentPage(1)">NEXT</button>
    <p class="mb-5">
      Page: {{ currentPage }} of {{ Math.ceil(character.length / postsPerPage) }}
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
      search: "", // search bar
      showModal: false,
      index: "",
      home: null, // info after fetching homeworld of a character
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
    };
    getPeople();
  },

  //   Dialog
  methods: {
    closeModal() {
      this.showModal = false;
    },

    toggleModal(index) {
      // Fetch info from Homeworld
      this.showModal = !this.showModal;
      let clickedCharacter = this.character[index]; // Info about the clicked post/ homeworld
      let fetchHomeworld = clickedCharacter.homeworld; // Querset to fetch homeworld of clicked character
      fetch(fetchHomeworld)
        .then((response) => response.json())
        .then((data) => {
          this.home = data; 
        });
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
    currentPagePosts() {
      // add a variable to query search character
      let searchChar = this.character.filter((blog) => {
        return blog.name.toLowerCase().includes(this.search.toLowerCase());
      });

      // add a variable to query pagination
      let paginate = this.character.slice(
        (this.currentPage - 1) * this.postsPerPage,
        this.currentPage * this.postsPerPage
      );

      let search = this.search;
      if (search) {
        return searchChar;
      } else {
        return paginate;
      }
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

/* Dialog or Modal */
.dialog {
  position: relative;
  width: 60%;
  padding: 20px;
  margin: 100px auto;
  background: white;
  border-radius: 10px;
  border-bottom: 6px solid #EE7007;

}

.backdrop {
  top: 0;
  position: fixed;
  background: rgba(0, 0, 0, 0.5);
  width: 100%;
  height: 100%;
}
.card {
  border-bottom: 6px solid #EE7007;
}

/* Input field */
input {
  text-align: center;
  border: .5px solid #EE7007;
  border-radius: 5px;
  height: 2.5em;
}

input:hover {
  border: 1px solid #c95806;
}

/* Button */
.btn {
  background-color: #EE7007;
  border: none;
  font-weight: 900;
}

.btn:hover {
  background-color: #c95806;
}

/* Pagination button */
.btn-pagination {
  background-color: #818a91;
}

.btn-pagination:hover {
  background-color: #55595d;
}


/* Put plcaeholder in the center */
::placeholder {
  text-align: center;
}

/* Modal Image */
.home-image {
    border-radius: 5px;
    width: 100%;
    height: 130px;
    position: absolute;
    transform: translate(-50%, -50%);
}

.home-name {
  position: relative;
  margin-bottom: 40px;
}

/* Media Query */
@media only screen and (max-width: 792px) {
  .dialog {
    width: 70%;
  }
}

@media only screen and (max-width: 600px) {
  .dialog {
    width: 70%;
  }
}
</style>