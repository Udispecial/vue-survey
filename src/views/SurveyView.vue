<template>
  <PageComponent>
    <template v-slot:header>
      <div class="flex items-center justify-between">
        <h1 class="text-3xl font-bold text-gray-900">
          {{ route.params.id ? model.title : "Create a Survey" }}
        </h1>
        <button
          v-if="route.params.id"
          type="button"
          @click="deleteSurvey()"
          class="py-2 px-3 text-white bg-red-500 rounded-md hover:bg-red-600"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-5 w-5 -mt-1 inline-block"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
            stroke-width="2"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"
            />
          </svg>
          Delete Survey
        </button>
      </div>
    </template>
    <!-- <div v-if="surveyLoading" class="flex justify-center">Loading...</div> -->
    <Loading v-if="surveyLoading" />
    <form v-else @submit.prevent="saveSurvey" class="animate-fade-in-down">
      <div class="shadow sm:rounded-md sm:overflow-hidden">
        <!-- survey fields -->
        <div class="px-4 py-5 bg-white space-y-6 sm:p-6">
          <!-- image  -->
          <div>
            <label class="block text-sm font-medium text-gray-700">
              Image
            </label>
            <div class="mt-1 flex items-center">
              <img
                v-if="model.image_url"
                :src="model.image_url"
                :alt="model.title"
                class="w-64 h-48 object-cover"
              />
              <span
                v-else
                class="flex items-center justify-center w-14 h-14 rounded-full overflow-hidden bg-gray-100"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-[80%] w-[80%] text-gray-300"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                  stroke-width="2"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z"
                  /></svg
              ></span>
              <button
                type="button"
                class="relative overflow-hidden ml-5 bg-white py-2 px-3 border border-gray-300 rounded-md shadow-sm text-sm leading-4 font-medium text-gray-700 hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
              >
                <input
                  type="file"
                  @change="onImageChoose"
                  class="absolute left-0 right-0 top-0 bottom-0 opacity-0 cursor-pointer"
                />
                Change
              </button>
            </div>
          </div>

          <!-- title  -->
          <div>
            <label for="title" class="block text-sm font-medium text-gray-700"
              >Title</label
            >
            <input
              type="text"
              name="title"
              id="title"
              v-model="model.title"
              autocomplete="survey_title"
              class="mt-1 focus:ring-indigo-500 focus:border-indigo-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md"
            />
          </div>
          <!-- description  -->
          <div>
            <label
              for="description"
              class="block text-sm font-medium text-gray-700"
              >Description</label
            >
            <textarea
              type="text"
              rows="3"
              name="description"
              id="description"
              v-model="model.description"
              autocomplete="survey_description"
              placeholder="describe your survey"
              class="mt-1 focus:ring-indigo-500 focus:border-indigo-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md"
            />
          </div>
          <!-- expire date  -->
          <div>
            <label
              for="expire_date"
              class="block text-sm font-medium text-gray-700"
              >Expire Date</label
            >
            <input
              type="date"
              name="expire_date"
              id="expire_date"
              v-model="model.expire_date"
              class="mt-1 focus:ring-indigo-500 focus:border-indigo-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md"
            />
          </div>

          <!-- status  -->
          <div class="flex items-start">
            <div class="flex items-center h-5">
              <input
                type="checkbox"
                id="status"
                name="status"
                v-model="model.status"
                class="focus:ring-indigo-500 h-4 w-4 text-indigo-600 border-gray-300 rounded"
              />
            </div>
            <div class="ml-3 text-sm">
              <label for="status" class="font-medium text-gray-700"
                >Active</label
              >
            </div>
          </div>

          <!-- questions  -->
          <div class="px-4 py-5 bg-white space-y-6 sm:p-6">
            <h3
              class="text-2xl font-semibold flex items-center justify-between"
            >
              Questions
              <!-- add new question  -->
              <button
                type="button"
                @click="addQuestion()"
                class="flex items-center text-sm py-1 px-4 rounded-sm text-white bg-gray-600 hover:bg-gray-700"
              >
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-4 w-4"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                  stroke-width="2"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M12 4v16m8-8H4"
                  />
                </svg>
                Add Question
              </button>
            </h3>
            <div
              v-if="!model.question.length"
              class="text-center text-gray-600"
            >
              You don't have any questions created
            </div>
            <div v-for="(question, index) in model.question" :key="question.id">
              <QuestionEditor
                @change="questionChange"
                @addQuestion="addQuestion"
                @deleteQuestion="deleteQuestion"
                :question="question"
                :index="index"
              />
            </div>
          </div>

          <!-- form footer  -->
          <div class="px-4 py-3 bg-gray-50 text-right sm:px-6">
            <button
              type="button"
              @click="saveSurvey()"
              class="inline-flex justify-center py-2 px-4 border border-transparent shadow-sm text-sm font-medium rounded-md text-write bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
            >
              Save
            </button>
          </div>
        </div>
      </div>
    </form>
  </PageComponent>
</template>

<script setup>
import { v4 as uuidv4 } from "uuid";
import PageComponent from "../components/PageComponent.vue";
import QuestionEditor from "../components/editor/QuestionEditor.vue";
import { computed, ref, watch } from "vue";
import store from "../store";
import { useRoute, useRouter } from "vue-router";
import Loading from "../components/Loading.vue";

const router = useRouter();
const route = useRoute();

const surveyLoading = computed(() => store.state.currentSurvey.loading);

//create empty survey
let model = ref({
  title: "",
  status: false,
  description: null,
  image_url: null,
  expire_date: null,
  question: [],
});
// watch currentSurvey data change and update the local model variable
watch(
  () => store.state.currentSurvey.data,
  (newVal) => {
    if (newVal) {
      model.value = {
        ...JSON.parse(JSON.stringify(store.state.currentSurvey.data)),
        status: newVal.status !== "draft",
      };
    }
  }
);
if (route.params.id) {
  store.dispatch("getSurvey", route.params.id);
}
function onImageChoose(ev) {
  const file = ev.target.files[0];

  const reader = new FileReader();
  reader.onload = () => {
    // the field to send on backend and apply validation
    model.value.image = reader.result;

    // the field to display
    model.value.image_url = reader.result;
  };
  reader.readAsDataURL(file);
}
function addQuestion(index) {
  const newQuestion = {
    id: uuidv4(),
    type: "text",
    question: "",
    description: null,
    data: {},
  };
  model.value.question.splice(index, 0, newQuestion);
}
function deleteQuestion(question) {
  model.value.question = model.value.question.filter(
    (q) => q.id !== question.id
  );
}

function questionChange(question) {
  model.value.question = model.value.question.map((q) => {
    if (q.id === question.id) {
      return JSON.parse(JSON.stringify(question));
    }
    return q;
  });
}

// create or update survey
function saveSurvey() {
  store.dispatch("saveSurvey", model.value).then(({ data }) => {
    store.commit("notify", {
      type: "success",
      message: "Survey was successfully updated",
    });
    router.push({
      name: "SurveyView",
      params: { id: data.data.id },
    });
  });
}
// delete survey
function deleteSurvey() {
  if (confirm("Are you sure you want to delete this survey??")) {
    store.dispatch("deleteSurvey", model.value.id).then(() => {
      router.push({
        name: "Surveys",
      });
    });
  }
}
</script>

<style scoped></style>
