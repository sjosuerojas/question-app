<template>
  <img :src="isValidQuestion ? image : defaultImg" alt="bg" />
  <div class="bg-dark"></div>

  <div class="maybe-container">
    <input
      v-model="question"
      type="text"
      placeholder="¡Hazme una pregunta guapo!"
    />
    <p>Recuerda terminar la pregunta con un (?)</p>

    <div v-if="isValidQuestion">
      <h2>{{ question }}</h2>
      <h1>{{ answer }}</h1>
    </div>
  </div>
</template>

<script>
import initialImage from "../assets/initialImage.jpg";

export default {
  name: "Question",
  data() {
    return {
      image: null,
      defaultImg: initialImage,
      question: null,
      answer: null,
      isValidQuestion: false,
    };
  },
  watch: {
    question(currentValue) {
      this.isValidQuestion = false;
      if (!currentValue.includes("?")) return;

      this.isValidQuestion = true;
      this.fetchAnswer();
    },
  },
  methods: {
    async fetchAnswer() {
      const response = await fetch("https://yesno.wtf/api");
      const { answer, image } = await response.json();

      this.answer = answer === "yes" ? "Obvio bobis!" : "No, que te pasa!";
      this.image = image;
    },
  },
};
</script>

<style sccoped>
img,
.bg-dark {
  height: 100vh;
  max-height: 100%;
  max-width: 100%;
  position: fixed;
  top: 0px;
  width: 100%;
  right: 0;
  left: 0;
  margin: 0 auto;
}

.bg-dark {
  background-color: rgb(51 51 51 / 70%);
}

.maybe-container {
  position: relative;
  z-index: 99;
}

input {
  width: 250px;
  padding: 10px 15px;
  border-radius: 5px;
  border: none;
}
input:focus {
  outline: none;
}

p {
  color: white;
  font-size: 20px;
}

h1,
h2 {
  color: white;
}

h2 {
  margin-top: 150px;
}
</style>
