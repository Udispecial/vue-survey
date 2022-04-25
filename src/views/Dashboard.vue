<template>
  <div>
    <PageComponent title="Dashboard">
      <Loading v-if="loading" />
      <div
        v-else
        class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-5 text-gray-700"
      >
        <div
          class="bg-white shadow-md p-3 text-center flex flex-col order-1 lg:order-2 animate-fade-in-down"
          style="animation-delay: 0.1s"
        >
          <h3 class="font-semibold text-2xl">Total Surveys</h3>
          <div
            class="text-8xl font-semibold flex-1 flex items-center justify-center"
          >
            {{ data.totalSurveys }}
          </div>
        </div>
        <div
          class="bg-white shadow-md p-3 text-center flex flex-col order-2 lg:order-4 animate-fade-in-down"
          style="animation-delay: 0.2s"
        >
          <h3 class="font-semibold text-2xl">Total Answers</h3>
          <div
            class="text-8xl font-semibold flex-1 flex items-center justify-center"
          >
            {{ data.totalAnswers }}
          </div>
        </div>
        <div
          class="bg-white shadow-md p-4 row-span-2 order-3 lg:order-1 animate-fade-in-down"
        >
          <h3 class="font-semibold text-2xl">Latest Survey</h3>
          <img
            :src="data.latestSurvey.image_url"
            class="w-[240px] mx-auto"
            alt=""
          />
          <h3 class="font-bold text-xl mb-3">{{ data.latestSurvey.title }}</h3>
          <div class="text-sm mb-1 flex justify-between">
            <div>Create Date:</div>
            <div>{{ data.latestSurvey.created_at }}</div>
          </div>
          <div class="text-sm mb-1 flex justify-between">
            <div>Expire Date:</div>
            <div>{{ data.latestSurvey.expire_date }}</div>
          </div>
          <div class="text-sm mb-1 flex justify-between">
            <div>Status:</div>
            <div>{{ data.latestSurvey.status ? "Active" : "Draft" }}</div>
          </div>
          <div class="text-sm mb-1 flex justify-between">
            <div>Questions:</div>
            <div>{{ data.latestSurvey.question }}</div>
          </div>
          <div class="text-sm mb-3 flex justify-between">
            <div>Answers:</div>
            <div>{{ data.latestSurvey.answer }}</div>
          </div>
          <div class="flex justify-between">
            <router-link
              :to="{ name: 'SurveyView', params: { id: data.latestSurvey.id } }"
              class="flex py-2 px-4 border border-transparent text-sm rounded-md text-indigo-500 hover:bg-indigo-700 hover:text-white transition-colors focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="h-5 w-5 mr-2"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
                stroke-width="2"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="M11 5H6a2 2 0 00-2 2v11a2 2 0 002 2h11a2 2 0 002-2v-5m-1.414-9.414a2 2 0 112.828 2.828L11.828 15H9v-2.828l8.586-8.586z"
                />
              </svg>
              Edit Survey
            </router-link>
            <button
              class="flex py-2 px-4 border border-transparent text-sm rounded-md text-indigo-500 hover:bg-indigo-700 hover:text-white transition-colors focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="h-5 w-5 mr-2"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
                stroke-width="2"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"
                />
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z"
                />
              </svg>
              View Answer
            </button>
          </div>
        </div>
        <div
          class="bg-white shadow-md p-3 row-span-2 order-4 lg:order-3 animate-fade-in-down"
          style="animation-delay: 0.3s"
        >
          <div class="flex justify-between items-center mb-3 px-2">
            <h3 class="font-semibold text-2xl">Latest Answer</h3>
            <a
              href="javascript:void(0)"
              class="text-sm text-blue-500 hover:decoration-blue-500"
            >
              View All
            </a>
          </div>
          <a
            href="#"
            v-for="answer of data.latestAnswers"
            :key="answer.id"
            class="block p-2 hover:bg-gray-100/90"
          >
            <div class="font-semibold">{{ answer.survey.title }}</div>
            <small>
              Answer made at:
              <i class="font-semibold">{{ answer.end_date }}</i>
            </small>
          </a>
        </div>
      </div>
    </PageComponent>
  </div>
</template>

<script setup>
import PageComponent from "../components/PageComponent.vue";
import Loading from "../components/Loading.vue";
import { computed } from "vue";
import { useStore } from "vuex";

const store = useStore();
const loading = computed(() => store.state.dashboard.loading);
const data = computed(() => store.state.dashboard.data);

store.dispatch("getDashboardData");
</script>

<style scoped></style>
