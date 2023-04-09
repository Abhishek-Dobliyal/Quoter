<template>
  <div class="d-flex align-items-center justify-content-center p-5">
    <div class="container border-2 text-center mt-5">
      <div class="row d-flex justify-content-center align-items-center">
        <div class="col col-lg-9 col-xl-7">
          <div class="card" style="border-radius: 15px">
            <div class="card-body p-5">
              <div class="text-center mb-4 pb-2">
                <img
                  src="https://mdbcdn.b-cdn.net/img/Photos/new-templates/bootstrap-quotes/bulb.webp"
                  alt="Bulb"
                  width="100"
                />
              </div>

              <figure class="text-center mb-0">
                <blockquote class="blockquote">
                  <p class="pb-3">
                    <i class="fas fa-quote-left fa-xs text-primary"></i>
                    <span class="lead font-italic quote-font">{{
                      quote.content
                    }}</span>
                    <i class="fas fa-quote-right fa-xs text-primary"></i>
                  </p>
                </blockquote>
                <figcaption class="blockquote-footer mb-0">
                  {{ quote.author }}
                </figcaption>
              </figure>
              <button
                class="btn btn-primary mt-4 fetch py-2 px-4"
                @click="fetchQuote"
              >
                <i
                  class="fa-solid fa-arrow-right text-light"
                  v-if="!isLoading"
                ></i>
                <i class="fa-solid fa-spinner fa-spin text-light" v-else></i>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { onMounted, reactive, ref } from "vue";

const apiUrl = "https://api.quotable.io/quotes/random?maxLength=100&minLength=75";
const heading = ref("Quoter");
const subHeading = ref("Get Inspired with Every Click");
const isLoading = ref(false);
const gradient = [
  "linear-gradient(lightcyan, lightpink)",
  "linear-gradient(lightblue, lightpink)",
  "linear-gradient(lightcoral, lightgreen)",
  "linear-gradient(lightseagreen, lightsteelblue)",
];

const quote = reactive({
  content: "",
  author: "",
});

onMounted(() => {
  fetchQuote();
});

const fetchQuote = () => {
  isLoading.value = true;
  axios
    .get(apiUrl)
    .then((res) => {
      const data = res.data[0];
      quote.content = data.content;
      quote.author = data.author;
      isLoading.value = false;
    })
    .catch((err) => {
      console.log(err);
    });

  document.body.style.background = getRandomGradient();
};

const getRandomGradient = () => {
  return gradient[Math.floor(Math.random() * gradient.length)];
};
</script>

<style>
html {
  height: 100%;
}
body {
  height: 100%;
  margin: 0;
  background-repeat: no-repeat;
  background-attachment: fixed;
  background: linear-gradient(lightcyan, lightpink);
  height: 100%;
  font-family: "Varela Round", sans-serif;
}

hr {
  height: 3px;
  border: none;
  background-color: black;
}

p.lead {
  color: darkcyan;
}

span.quote-font {
  font-family: "Handlee", cursive;
}

h1 {
  text-shadow: 0 0 5px black, 0 0 5px darkgoldenrod;
}
</style>
