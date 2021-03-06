<template>
  <div class="px-5 py-8">
    <Loading v-if="loading" />
    <form @submit.prevent="submitSurvey" v-else class="container mx-auto">
      <div class="grid grid-cols-6 items-center">
        <div class="mr-4">
          <img :src="survey.image_url" alt="" />
        </div>
        <div class="col-span-5">
          <h1 class="text-3xl mb-3">{{ survey.title }}</h1>
          <p class="text-gray-500 text-sm" v-html="survey.description"></p>
        </div>
      </div>

      <div
        v-if="surveyFinished"
        class="py-8 px-6 bg-emerald-400 text-white w-[600px] mx-auto"
      >
        <div class="text-xl mb-3 font-semibold">
          Thanks for participating in this survey
        </div>
        <button
          @click="submitAnotherResponse"
          type="submit"
          class="inline-flex justify-center py-2 px-4 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
        >
          Submit another response
        </button>
      </div>
      <div v-else>
        <hr class="my-3" />
        <div v-for="(question, ind) of survey.question" :key="question.id">
          <QuestionViewer
            v-model="answers[question.id]"
            :question="question"
            :index="ind"
          />
        </div>
      </div>

      <button
        type="submit"
        class="inline-flex justify-center py-2 px-4 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
      >
        Submit
      </button>
    </form>
  </div>
</template>

<script setup>
import { computed, ref } from "vue";
import { useRoute } from "vue-router";
import { useStore } from "vuex";
import Loading from "../components/Loading.vue";
import QuestionViewer from "../components/viewer/QuestionViewer.vue";
const route = useRoute();
const store = useStore();
const loading = computed(() => store.state.currentSurvey.loading);
const survey = computed(() => store.state.currentSurvey.data);

const surveyFinished = ref(false);
const answers = ref({});

store.dispatch("getSurveyBySlug", route.params.slug);

function submitSurvey() {
  console.log(JSON.stringify(answers.value, undefined, 2));
  store
    .dispatch("saveSurveyAnswers", {
      surveyId: survey.value.id,
      answer: answers.value,
    })
    .then((response) => {
      if (response.status === 201) {
        surveyFinished.value = true;
      }
    });
}

function submitAnotherResponse() {
  answers.value = {};
  surveyFinished.value = false;
}
</script>

<style scoped></style>
