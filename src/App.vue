<template>
  <div
    class="justify-center flex items-center h-screen bg-gradient-to-t from-zinc-900 via-zinc-800 to-zinc-700">
    <div class="w-2/3 h-fit bg-white rounded-lg flex flex-col text-center p-4">
      <!-- Title -->
      <div class="w-full h-fit text-4xl font-bold my-10">AICare Survey</div>

      <!-- Question Display -->
      <div class="flex-1 flex items-center justify-center">
        <div v-if="currentQuestion" class="text-2xl">
          {{ currentQuestion.question }}
        </div>
        <div v-else-if="loading" class="text-2xl">Loading...</div>
        <div v-else class="text-2xl">No questions available</div>
      </div>

      <!-- Buttons -->
      <div class="my-10" v-if="currentQuestion">
        <button
          @click="submitAnswer"
          class="text-white px-6 py-2 rounded-lg bg-black">
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
