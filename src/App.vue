<template>
  <div
    class="justify-center flex items-center h-screen bg-gradient-to-b from-fuchsia-900 via-black to-black">
    <div
      class="w-2/3 h-[75%] bg-white rounded-lg flex flex-col text-center p-4">
      <!-- Title -->
      <div class="w-full h-fit text-4xl font-bold">AICare Survey</div>

      <!-- Question Display -->
      <div class="flex-1 flex items-center justify-center">
        <div v-if="currentQuestion" class="text-2xl">
          {{ currentQuestion.question }}
        </div>
        <div v-else-if="loading" class="text-2xl">Loading...</div>
        <div v-else class="text-2xl">No questions available</div>
      </div>

      <!-- Buttons -->
      <div class="mt-4" v-if="currentQuestion">
        <button
          @click="submitAnswer"
          class="bg-blue-500 text-white px-6 py-2 rounded-lg hover:bg-blue-700">
          Submit
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      questions: [],
      currentIndex: 0,
      loading: true,
    };
  },
  computed: {
    currentQuestion() {
      return this.questions[this.currentIndex] || null;
    },
  },
  methods: {
    async fetchQuestions() {
      try {
        const response = await fetch("public/question.json");
        this.questions = await response.json();
        this.loading = false;
      } catch (error) {
        console.error("Error fetching questions:", error);
        this.loading = false;
      }
    },
    submitAnswer() {
      if (this.currentIndex < this.questions.length - 1) {
        this.currentIndex++;
      } else {
        alert("Survey complete!");
      }
    },
  },
  mounted() {
    this.fetchQuestions();
    this.loading = false;
  },
};
</script>
