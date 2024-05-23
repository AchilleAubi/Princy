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
const itemsPerPage = 9;
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
    <div class="w-1/4 bg-slate-100 border border-gray-200 shadow">
      <div class="p-4">
        <h2><b>To do vue Js</b></h2>
        <form class="flex items-center max-w-sm mx-auto mt-4">
          <label for="simple-search" class="sr-only">Search</label>
          <div class="relative w-full">
            <div
              class="absolute inset-y-0 start-0 flex items-center ps-3 pointer-events-none"
            >
              <svg
                class="w-4 h-4 text-gray-500 dark:text-gray-400"
                aria-hidden="true"
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 20 20"
              >
                <path
                  stroke="currentColor"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="m19 19-4-4m0-7A7 7 0 1 1 1 8a7 7 0 0 1 14 0Z"
                />
              </svg>
            </div>
            <input
              v-model="searchKey"
              type="text"
              id="simple-search"
              class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full ps-10 p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
              placeholder="Search branch name..."
              required
            />
          </div>
        </form>
      </div>
      <div>
        <div class="p-4">
          <h2 class="text-blue-700 text-start"><b>Favoris</b></h2>
          <ul class="p-4">
            <li class="text-start">Mes tâches aujourd'hui</li>
            <li class="text-start">
              Important
              <span
                class="inline-flex items-center justify-center w-4 h-4 ms-2 text-xs font-semibold text-red-800 bg-red-200 rounded-full"
              >
                1
              </span>
            </li>
            <li class="text-start">Personnel</li>
            <li class="text-start">Toutes les tâches</li>
            <li class="text-start">Términés</li>
          </ul>
        </div>
      </div>
      <div>
        <div class="p-4">
          <h2 class="text-blue-700 text-start"><b>Tags</b></h2>
          <ul class="p-4">
            <li
              v-for="(tag, index) in tagsList"
              :key="index"
              class="text-start"
            >
              <span
                v-if="index % 2 === 0"
                class="inline-flex items-center justify-center w-4 h-4 ms-2 text-xs font-semibold text-red-800 bg-red-800 rounded-full"
              >
              </span>
              <span
                v-if="index % 2 !== 0"
                class="inline-flex items-center justify-center w-4 h-4 ms-2 text-xs font-semibold text-red-800 bg-yellow-400 rounded-full"
              >
              </span>
              {{ tag["name"] }}
            </li>
          </ul>
        </div>
      </div>
    </div>
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
      <div
        class="flex w-full p-4"
        v-for="company in filteredAndPaginatedCompanies.data"
        :key="company['task']"
      >
        <div class="w-1/4">
          <input type="checkbox" :checked="company['end-task']" />
        </div>
        <div class="w-1/2">
          <h5 class="card-title">{{ company["task"] }}</h5>
          <span><input type="radio" checked /></span>
          <span class="text-muted small pt-1 fw-bold"><b> Devs - </b></span>
          <span class="text-info small pt-2 ps-1">{{ company["date"] }}</span>
        </div>
        <div class="w-1/4">
          <i v-if="company['important']"
            ><svg
              class="w-4 h-4 text-yellow-300 me-1"
              aria-hidden="true"
              xmlns="http://www.w3.org/2000/svg"
              fill="currentColor"
              viewBox="0 0 22 20"
            >
              <path
                d="M20.924 7.625a1.523 1.523 0 0 0-1.238-1.044l-5.051-.734-2.259-4.577a1.534 1.534 0 0 0-2.752 0L7.365 5.847l-5.051.734A1.535 1.535 0 0 0 1.463 9.2l3.656 3.563-.863 5.031a1.532 1.532 0 0 0 2.226 1.616L11 17.033l4.518 2.375a1.534 1.534 0 0 0 2.226-1.617l-.863-5.03L20.537 9.2a1.523 1.523 0 0 0 .387-1.575Z"
              /></svg
          ></i>
          <i v-if="!company['important']"
            ><svg
              class="w-4 h-4 text-gray-300 me-1 dark:text-gray-500"
              aria-hidden="true"
              xmlns="http://www.w3.org/2000/svg"
              fill="currentColor"
              viewBox="0 0 22 20"
            >
              <path
                d="M20.924 7.625a1.523 1.523 0 0 0-1.238-1.044l-5.051-.734-2.259-4.577a1.534 1.534 0 0 0-2.752 0L7.365 5.847l-5.051.734A1.535 1.535 0 0 0 1.463 9.2l3.656 3.563-.863 5.031a1.532 1.532 0 0 0 2.226 1.616L11 17.033l4.518 2.375a1.534 1.534 0 0 0 2.226-1.617l-.863-5.03L20.537 9.2a1.523 1.523 0 0 0 .387-1.575Z"
              /></svg
          ></i>
        </div>
      </div>
      <br />
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
    <div class="w-1/4 bg-white border border-gray-200 shadow">
      <div
        class="flex text-center items-center md:order-2 space-x-3 md:space-x-0 rtl:space-x-reverse"
      >
        <img
          class="w-8 h-8 rounded-full"
          src="../assets/images.png"
          alt="user photo"
        />
        <div class="px-4 py-3">
          <span class="block text-sm text-gray-900 dark:text-white"
            >Bonnie Green</span
          >
          <span class="block text-sm text-gray-500 truncate dark:text-gray-400"
            >name@flowbite.com</span
          >
        </div>
      </div>

      <div
        class="w-full max-w-sm bg-gray-100 border border-gray-200 rounded-lg shadow dark:bg-gray-800 dark:border-gray-700 p-4"
      >
        <div class="flex justify-end px-4 pt-4 p-4">
          <button
            id="dropdownButton"
            data-dropdown-toggle="dropdown"
            class="inline-block text-gray-100 bg-gray-100 hover:bg-gray-100"
            type="button"
          >
            <span class="sr-only">Open dropdown</span>
            <svg
              class="w-5 h-5"
              aria-hidden="true"
              xmlns="http://www.w3.org/2000/svg"
              fill="currentColor"
              viewBox="0 0 16 3"
            >
              <path
                d="M2 0a1.5 1.5 0 1 1 0 3 1.5 1.5 0 0 1 0-3Zm6.041 0a1.5 1.5 0 1 1 0 3 1.5 1.5 0 0 1 0-3ZM14 0a1.5 1.5 0 1 1 0 3 1.5 1.5 0 0 1 0-3Z"
              />
            </svg>
          </button>
        </div>
        <div class="flex flex-col items-center pb-10">
          <h5 class="mb-1 text-xl font-medium text-gray-900 dark:text-white">
            Calendrier
          </h5>
          <span class="text-sm text-gray-500 dark:text-gray-400"></span>
          <div class="flex mt-4 md:mt-6">
            <a
              href="#"
              class="inline-flex items-center px-4 py-2 text-sm font-medium text-center text-white bg-gray-100 rounded-lg hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
            ></a>
          </div>
        </div>
      </div>

      <div>
        <div class="p-4">
          <h2 class="text-blue-700 text-start"><b>Taches terminées</b></h2>
          <ul class="p-4">
            <li
              v-for="(list, index) in todeList"
              :key="index"
              class="text-start"
            >
              <span
                v-if="list['end-task'] === true"
                class="inline-flex items-center justify-center w-4 h-4 ms-2 text-xs font-semibold text-red-800 bg-red-800 rounded-full"
              >
              </span>
              <span
                v-if="list['end-task'] === true"
                class="text-muted small pt-1 fw-bold"
                ><b> Devs - </b></span
              >
              <span
                v-if="list['end-task'] === true"
                class="text-info small pt-2 ps-1"
                >{{ list["task"] }}</span
              >
            </li>
          </ul>
        </div>
      </div>
    </div>
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
