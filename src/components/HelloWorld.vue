<script setup lang="ts">
import { onMounted, ref, computed } from "vue";
import {
  initAccordions,
  initCarousels,
  initCollapses,
  initDials,
  initDismisses,
  initDrawers,
  initDropdowns,
  initModals,
  initPopovers,
  initTabs,
  initTooltips,
  initFlowbite,
} from "flowbite";
import axios from "axios";

onMounted(() => {
  initAccordions();
  initCarousels();
  initCollapses();
  initDials();
  initDismisses();
  initDrawers();
  initDropdowns();
  initModals();
  initPopovers();
  initTabs();
  initTooltips();
  initFlowbite();
  getData();
});

const todeList = ref<any[]>([]);
const tagsList = ref<any[]>([]);
const currentPage = ref(1);
const itemsPerPage = 8;
const searchKey = ref("");

async function getData() {
  try {
    const response = await axios.get(
      "https://retoolapi.dev/9lwmoL/to-do-edisys"
    );
    todeList.value = response.data;
    const tag = await axios.get("https://retoolapi.dev/kEbZ3j/tags");
    tagsList.value = tag.data;
  } catch (error) {
    console.error("Une erreur s'est produite:", error);
  }
}

const filteredAndPaginatedCompanies = computed<{
  totalPages: number;
  data: any[];
}>(() => {
  const filtered = todeList.value.filter(
    (company) =>
      company["task"] &&
      company["task"].toLowerCase().includes(searchKey.value.toLowerCase())
  );

  if (filtered.length === 0) {
    return {
      totalPages: 0,
      data: [],
    };
  }

  const startIndex = (currentPage.value - 1) * itemsPerPage;
  const endIndex = startIndex + itemsPerPage;

  const totalPages = Math.ceil(filtered.length / itemsPerPage);
  const data = filtered.slice(startIndex, endIndex);

  return {
    totalPages,
    data,
  };
});

function prevPage() {
  if (currentPage.value > 1) {
    currentPage.value--;
  }
}

function nextPage() {
  if (currentPage.value < filteredAndPaginatedCompanies.value.totalPages) {
    currentPage.value++;
  }
}

function setPage(pageNumber: number) {
  currentPage.value = pageNumber;
}
</script>

<template>
  <div class="flex">
    <div class="w-1/4 bg-slate-100 border border-gray-200 shadow"></div>
    <div class="w-1/2 p-6 bg-white border border-gray-200 shadow">
      <div class="flex pl-4">
        <div class="w-1/2">
          <div class="flex justify-start pt-4">
            <h2>
              <b>Aujourd'hui</b>
            </h2>
          </div>
          <div class="flex justify-start">
            <p class="font-serif text-blue-700"><b>23 Mai 2024</b></p>
          </div>
        </div>
        <div class="w-1/2">
          <div>
            <div class="flex justify-end p-4">
              <button
                id="button"
                data-modal-toggle="modal"
                data-modal-target="modal"
                type="button"
                class="text-blue-700 bg-blue-200 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center"
              >
                + Creer
              </button>
            </div>

            <div
              id="modal"
              tabindex="-1"
              aria-hidden="true"
              class="fixed top-0 left-0 right-0 z-50 hidden w-full p-4 overflow-x-hidden overflow-y-auto md:inset-0 h-[calc(100%-1rem)] max-h-full"
            >
              <div class="relative w-full max-w-2xl max-h-full">
                <!-- Modal content -->
                <div
                  class="relative bg-white rounded-lg shadow dark:bg-gray-700"
                >
                  <!-- Modal header -->
                  <div
                    class="flex items-start justify-between p-5 border-b rounded-t dark:border-gray-600"
                  >
                    <h3
                      class="text-xl font-semibold text-gray-900 lg:text-2xl dark:text-white"
                    >
                      Créer une tâche
                    </h3>
                    <button
                      id="closeButton"
                      data-modal-hide="modal"
                      type="button"
                      class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm p-1.5 ml-auto inline-flex items-center dark:hover:bg-gray-600 dark:hover:text-white"
                    >
                      <svg
                        class="w-5 h-5"
                        fill="currentColor"
                        viewBox="0 0 20 20"
                        xmlns="http://www.w3.org/2000/svg"
                      >
                        <path
                          fill-rule="evenodd"
                          d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
                          clip-rule="evenodd"
                        ></path>
                      </svg>
                    </button>
                  </div>
                  <!-- Modal body -->
                  <div class="p-6 space-y-6">
                    <form class="max-w-sm mx-auto">
                      <div class="mb-5">
                        <label
                          for="email"
                          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white text-start"
                          >Date</label
                        >
                        <input
                          type="text"
                          id="date"
                          class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                          required
                        />
                      </div>
                      <div class="mb-5">
                        <label
                          for="password"
                          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white text-start"
                          >Nom</label
                        >
                        <input
                          type="text"
                          id="name"
                          class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                          required
                        />
                      </div>
                      <div class="mb-5">
                        <label
                          for="password"
                          class="block mb-2 text-sm font-medium text-gray-900 dark:text-white text-start"
                          >Tag</label
                        >
                        <input
                          type="text"
                          id="tag"
                          class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                          required
                        />
                      </div>
                      <div class="mb-5">
                        <button
                          type="submit"
                          class="text-blue-700 bg-blue-200 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm w-full px-5 py-2.5 text-center"
                        >
                          Creer
                        </button>
                      </div>
                    </form>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="flex w-full">
        <div style="margin-top: 5%">
          <div
            v-for="company in filteredAndPaginatedCompanies.data"
            :key="company['task']"
          >
            <div class="card-body">
              <div class="row">
                <div class="col-md-1 text-center card-title">
                  <input type="checkbox" :checked="company['end-task']" />
                </div>
                <div class="col-md-10">
                  <h5 class="card-title">{{ company["task"] }}</h5>
                  <span><input type="radio" checked /></span>
                  <span class="text-muted small pt-1 fw-bold"
                    ><b> Devs - </b></span
                  >
                  <span class="text-info small pt-2 ps-1">{{
                    company["date"]
                  }}</span>
                </div>
                <div class="col-md-1 text-center card-title">
                  <i v-if="company['important']" class="bi bi-star-fill"></i>
                  <i v-if="!company['important']" class="bi bi-star"></i>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="flex">
        <nav aria-label="Page navigation example">
          <ul class="flex items-center -space-x-px h-8 text-sm">
            <li :class="{ disabled: currentPage === 1 }">
              <a
                href="#"
                @click="prevPage"
                class="flex items-center justify-center px-3 h-8 ms-0 leading-tight text-gray-500 bg-white border border-e-0 border-gray-300 rounded-s-lg hover:bg-gray-100 hover:text-gray-700 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-white"
              >
                <span class="sr-only">Previous</span>
                <svg
                  class="w-2.5 h-2.5 rtl:rotate-180"
                  aria-hidden="true"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 6 10"
                >
                  <path
                    stroke="currentColor"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M5 1 1 5l4 4"
                  />
                </svg>
              </a>
            </li>
            <li
              v-for="nbrpage in filteredAndPaginatedCompanies.totalPages"
              :key="nbrpage"
            >
              <a
                :class="{ active: currentPage === nbrpage }"
                @click="setPage(nbrpage)"
                class="flex items-center justify-center px-3 h-8 leading-tight text-gray-500 bg-white border border-gray-300 hover:bg-gray-100 hover:text-gray-700 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-white"
                >{{ nbrpage }}</a
              >
            </li>
            <li
              :class="{
                disabled:
                  currentPage === filteredAndPaginatedCompanies.totalPages,
              }"
            >
              <a
                @click="nextPage"
                href="#"
                class="flex items-center justify-center px-3 h-8 leading-tight text-gray-500 bg-white border border-gray-300 rounded-e-lg hover:bg-gray-100 hover:text-gray-700 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-white"
              >
                <span class="sr-only">Next</span>
                <svg
                  class="w-2.5 h-2.5 rtl:rotate-180"
                  aria-hidden="true"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 6 10"
                >
                  <path
                    stroke="currentColor"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="m1 9 4-4-4-4"
                  />
                </svg>
              </a>
            </li>
          </ul>
        </nav>
      </div>
    </div>
    <div class="w-1/4 bg-white border border-gray-200 shadow"></div>
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}

.active {
  background-color: rgb(251 146 60);
}
</style>
