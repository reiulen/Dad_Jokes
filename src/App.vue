<template>
  <div id="app">
    <b-container>
      <b-row class="align-items-center justify-content-center">
        <b-col col lg="6">
          <b-card class="mb-2 text-end">
            <b-card-body>
              <SkeletonCard class="mb-2" v-if="loaderData" />
              <h5 class="mb-3" v-else>
                {{ data.setup }}
              </h5>

              <SkeletonCard v-if="loaderData" />
              <h6 v-else>
                {{ data.punchline }}
              </h6>

              <div class="mt-4">
                <b-button @click="getData" variant="danger">
                  Reload
                  <b-spinner small v-if="loaderData"></b-spinner>
                </b-button>
              </div>
            </b-card-body>
          </b-card>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import axios from "axios";
import SkeletonCard from "./components/SkeletonCard.vue";
export default {
  name: "App",
  components: {
    SkeletonCard,
  },
  data() {
    return {
      data: {},
      loaderData: true,
    };
  },
  methods: {
    async getData() {
      this.loaderData = true;
      const response = await axios.get(
        "https://official-joke-api.appspot.com/random_joke"
      );
      this.data = response.data;
      this.loaderData = false;
    },
  },
  created: function () {
    this.getData();
  },
};
</script>

<style>
body {
  background-color: #ffb6c1 !important;
}

#app {
  margin: 50px auto;
}

.skeleton-box {
  display: inline-block;
  height: 1em;
  position: relative;
  overflow: hidden;
  background-color: #f0f5fa;
}
.skeleton-box::after {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  transform: translateX(-100%);
  background-image: linear-gradient(
    90deg,
    rgba(rgb(255, 255, 255), 0) 0,
    rgba(rgb(255, 255, 255), 0.2) 20%,
    rgba(rgb(255, 255, 255), 0.5) 60%,
    rgba(rgb(255, 255, 255), 0)
  );
  animation: shimmer 2s infinite;
  content: "";
}
@keyframes shimmer {
  100% {
    transform: translateX(100%);
  }
}
</style>
