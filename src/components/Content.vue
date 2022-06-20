<script setup>
import { ref } from "vue";
import BarChart from "./BarChart.vue";

// import Swiper from "./Swiper.vue";

// export default {
//   name: 'Content',
//   components: { BarChart }
// }
// каждый 7 элемент массива новфй день  &units=metric
// chartjs
const userCity = ref("Ростов-на-Дону");

const error = ref(null);
const weatherAPI = ref(null);
// const tempAverage = ref(null);
const cityName = ref("");

// инфа по след дням
const week = [
  "Воскресенье",
  "Понедельник",
  "Вторник",
  "Среда",
  "Четверг",
  "Пятница",
  "Суббота",
];
const firstDay = ref({});
const secondDay = ref({});
const thirdDay = ref({});
const today = ref({});
const dataFirstDay = ref({});
const dataSecondDay = ref({});
const dataThirdDay = ref({});
const srcFirstDay = ref("");
const srcSecondDay = ref("");
const srcThirdDay = ref("");

function getWeatherAPI() {
  fetch(
    `https://api.openweathermap.org/data/2.5/forecast?q=${userCity.value}&lang=ru&units=metric&appid=86095446ea62c353a352f44e0b12a207`
  )
    .then((response) => response.json())
    .then((result) => {
      if (result.cod === "404" || result.cod === "400") {
        error.value = result.message;
      } else {
        weatherAPI.value = result;
        // src.value = `http://openweathermap.org/img/wn/${weatherAPI.value.list[0].weather[0].icon}@2x.png`;
        // tempAverage.value = Math.round(
        //   (weatherAPI.value.list[0].main.temp_min +
        //     weatherAPI.value.list[0].main.temp_max) /
        //     2
        // );
        today.value = weatherAPI.value.list[0];
        firstDay.value = weatherAPI.value.list[9];
        secondDay.value = weatherAPI.value.list[17];
        thirdDay.value = weatherAPI.value.list[25];
        let todayDayWeek = new Date(today.value.dt_txt.split(" ")[0]);
        let firstDayWeek = new Date(firstDay.value.dt_txt.split(" ")[0]);
        let SecondDayWeek = new Date(secondDay.value.dt_txt.split(" ")[0]);
        let ThirdDayWeek = new Date(thirdDay.value.dt_txt.split(" ")[0]);
        // console.log(weatherAPI.value.list[2].weather[0].icon)

        today.value = {
          numberDay: today.value.dt_txt.split("-")[2].split(" ")[0],
          month: today.value.dt_txt.split("-")[1],
          dayOfWeek: week[todayDayWeek.getDay()],
          src: `http://openweathermap.org/img/wn/${today.value.weather[0].icon}@2x.png`,
          src1: `http://openweathermap.org/img/wn/${weatherAPI.value.list[1].weather[0].icon}@2x.png`,
          src2: `http://openweathermap.org/img/wn/${weatherAPI.value.list[2].weather[0].icon}@2x.png`,
          src3: `http://openweathermap.org/img/wn/${weatherAPI.value.list[3].weather[0].icon}@2x.png`,
          src4: `http://openweathermap.org/img/wn/${weatherAPI.value.list[4].weather[0].icon}@2x.png`,
          src5: `http://openweathermap.org/img/wn/${weatherAPI.value.list[5].weather[0].icon}@2x.png`,
          src6: `http://openweathermap.org/img/wn/${weatherAPI.value.list[6].weather[0].icon}@2x.png`,
          src7: `http://openweathermap.org/img/wn/${weatherAPI.value.list[7].weather[0].icon}@2x.png`,
          dt1: weatherAPI.value.list[1].dt_txt
            .split(" ")[1]
            .split(":")
            .splice(0, 2)
            .join(":"),
          dt2: weatherAPI.value.list[2].dt_txt
            .split(" ")[1]
            .split(":")
            .splice(0, 2)
            .join(":"),
          dt3: weatherAPI.value.list[3].dt_txt
            .split(" ")[1]
            .split(":")
            .splice(0, 2)
            .join(":"),
          dt4: weatherAPI.value.list[4].dt_txt
            .split(" ")[1]
            .split(":")
            .splice(0, 2)
            .join(":"),
          dt5: weatherAPI.value.list[5].dt_txt
            .split(" ")[1]
            .split(":")
            .splice(0, 2)
            .join(":"),
          dt6: weatherAPI.value.list[6].dt_txt
            .split(" ")[1]
            .split(":")
            .splice(0, 2)
            .join(":"),
          dt7: weatherAPI.value.list[7].dt_txt
            .split(" ")[1]
            .split(":")
            .splice(0, 2)
            .join(":"),
          tempAverage:
            Math.round(
              weatherAPI.value.list[0].main.temp_min +
                weatherAPI.value.list[0].main.temp_max
            ) / 2,
        };
        console.log();
        dataFirstDay.value = {
          numberDay: firstDay.value.dt_txt.split("-")[2].split(" ")[0],
          month: firstDay.value.dt_txt.split("-")[1],
          dayOfWeek: week[firstDayWeek.getDay()],
          src: `http://openweathermap.org/img/wn/${firstDay.value.weather[0].icon}@2x.png`,
          tempAverage:
            Math.round(
              firstDay.value.main.temp_min + firstDay.value.main.temp_max
            ) / 2,
        };
        dataSecondDay.value = {
          numberDay: secondDay.value.dt_txt.split("-")[2].split(" ")[0],
          month: secondDay.value.dt_txt.split("-")[1],
          dayOfWeek: week[SecondDayWeek.getDay()],
          src: `http://openweathermap.org/img/wn/${secondDay.value.weather[0].icon}@2x.png`,
          tempAverage:
            Math.round(
              secondDay.value.main.temp_min + secondDay.value.main.temp_max
            ) / 2,
        };
        dataThirdDay.value = {
          numberDay: thirdDay.value.dt_txt.split("-")[2].split(" ")[0],
          month: thirdDay.value.dt_txt.split("-")[1],
          dayOfWeek: week[ThirdDayWeek.getDay()],
          src: `http://openweathermap.org/img/wn/${thirdDay.value.weather[0].icon}@2x.png`,
          tempAverage:
            Math.round(
              thirdDay.value.main.temp_min + thirdDay.value.main.temp_max
            ) / 2,
        };
        cityName.value = weatherAPI.value.city.name;
      }
    });
  error.value = null;
  // weatherAPI.value = null;
  userCity.value = "";
}
</script>

<script>
// Import Swiper Vue.js components
import { Swiper, SwiperSlide } from "swiper/vue";
// Import Swiper styles
import "swiper/css";

// import "swiper/css/pagination";
// import "./style.css";
// import { Pagination } from "swiper";
// import { Autoplay } from "swiper";
export default {
  components: {
    Swiper,
    SwiperSlide,
  },
  setup() {
    return {
      //  modules: Autoplay,
    };
  },
};
</script>

<template>
  <div
    class="bg-base-100 flex-grow transition-all flex lg:items-center lg:justify-center"
  >
    <div
      class="flex flex-col gap-4 p-4 max-w-[1200px] m-auto items-center lg:flex-row lg:justify-between lg:items-stretch"
    >
      <!-- форма запроса погоды -->
      <div
        class="self-center lg:self-auto bg-base-200 rounded-md flex flex-col items-center gap-4 shadow-md p-4 w-[340px]"
      >
        <h1 class="p-2 text-2xl font-bold">Погода сегодня</h1>
        <form class="flex" @submit.prevent="getWeatherAPI">
          <input
            type="text"
            placeholder="Введите название города"
            class="input input-bordered input-primary w-full max-w-xs rounded-r-none"
            v-model="userCity"
          />
          <button
            class="btn btn-primary border-l-0 rounded-l-none"
            type="submit"
            :disabled="userCity.length < 3"
          >
            <i class="fa-duotone fa-magnifying-glass"></i>
          </button>
        </form>
        <!-- текущий прогноз -->
        <div class="flex flex-col items-center gap-4" v-if="weatherAPI">
          <h3 class="text-center font-bold text-xl">{{ cityName }}</h3>
          <h5 class="">
            {{ today.numberDay }}.{{ today.month }}, {{ today.dayOfWeek }}
          </h5>
          <div
            class="bg-base-300 rounded-full flex items-center justify-center w-[100px] h-[100px]"
          >
            <img class="" :src="today.src" alt="cityName" />
          </div>

          <p class="font-bold">
            {{ Math.round(weatherAPI.list[0].main.temp) }}&deg;C,
            {{ weatherAPI.list[0].weather[0].description }}
          </p>
          <div class="flex items-center justify-center w-[300px]">
            <swiper
              :grab-cursor="true"
              :freeMode="true"
              :slidesPerView="2"
              :spaceBetween="1"
              class="mySwiper bg-base-300 p-2 shadow-md rounded-md mb-2"
            >
              <swiper-slide class="border-r-2 border-base-200">
                <div class="flex flex-col items-center justify-center text-sm">
                  <h3 class="opacity-70">Сейчас</h3>
                  <p class="font-bold">
                    {{ Math.round(weatherAPI.list[0].main.temp) }}&deg;C
                  </p>
                  <img :src="today.src" alt="temp" class="w-[50px] h-[50px]" />
                  <p
                    class="text-center flex items-center justify-center gap-2 font-semibold"
                  >
                    <i class="fa-duotone fa-windsock"></i
                    >{{ weatherAPI.list[0].wind.speed }} м/с
                  </p>
                </div>
              </swiper-slide>
              <swiper-slide class="border-r-2 border-base-200">
                <div class="flex flex-col items-center justify-center text-sm">
                  <h3 class="opacity-70">{{ today.dt1 }}</h3>
                  <p class="font-bold">
                    {{ Math.round(weatherAPI.list[1].main.temp) }}&deg;C
                  </p>
                  <img :src="today.src1" alt="temp" class="w-[50px] h-[50px]" />
                  <p
                    class="text-center flex items-center justify-center gap-2 font-semibold"
                  >
                    <i class="fa-duotone fa-windsock"></i
                    >{{ weatherAPI.list[1].wind.speed }} м/с
                  </p>
                </div>
              </swiper-slide>
              <swiper-slide class="border-r-2 border-base-200">
                <div class="flex flex-col items-center justify-center text-sm">
                  <h3 class="opacity-70">{{ today.dt2 }}</h3>
                  <p class="font-bold">
                    {{ Math.round(weatherAPI.list[2].main.temp) }}&deg;C
                  </p>
                  <img :src="today.src2" alt="temp" class="w-[50px] h-[50px]" />
                  <p
                    class="text-center flex items-center justify-center gap-2 font-semibold"
                  >
                    <i class="fa-duotone fa-windsock"></i
                    >{{ weatherAPI.list[2].wind.speed }} м/с
                  </p>
                </div>
              </swiper-slide>
              <swiper-slide class="border-r-2 border-base-200">
                <div class="flex flex-col items-center justify-center text-sm">
                  <h3 class="opacity-70">{{ today.dt3 }}</h3>
                  <p class="font-bold">
                    {{ Math.round(weatherAPI.list[3].main.temp) }}&deg;C
                  </p>
                  <img :src="today.src3" alt="temp" class="w-[50px] h-[50px]" />
                  <p
                    class="text-center flex items-center justify-center gap-2 font-semibold"
                  >
                    <i class="fa-duotone fa-windsock"></i
                    >{{ weatherAPI.list[3].wind.speed }} м/с
                  </p>
                </div>
              </swiper-slide>
              <swiper-slide class="border-r-2 border-base-200">
                <div class="flex flex-col items-center justify-center text-sm">
                  <h3 class="opacity-70">{{ today.dt4 }}</h3>
                  <p class="font-bold">
                    {{ Math.round(weatherAPI.list[4].main.temp) }}&deg;C
                  </p>
                  <img :src="today.src4" alt="temp" class="w-[50px] h-[50px]" />
                  <p
                    class="text-center flex items-center justify-center gap-2 font-semibold"
                  >
                    <i class="fa-duotone fa-windsock"></i
                    >{{ weatherAPI.list[4].wind.speed }} м/с
                  </p>
                </div></swiper-slide
              >
              <swiper-slide class="border-r-2 border-base-200">
                <div class="flex flex-col items-center justify-center text-sm">
                  <h3 class="opacity-70">{{ today.dt5 }}</h3>
                  <p class="font-bold">
                    {{ Math.round(weatherAPI.list[5].main.temp) }}&deg;C
                  </p>
                  <img :src="today.src5" alt="temp" class="w-[50px] h-[50px]" />
                  <p
                    class="text-center flex items-center justify-center gap-2 font-semibold"
                  >
                    <i class="fa-duotone fa-windsock"></i
                    >{{ weatherAPI.list[5].wind.speed }} м/с
                  </p>
                </div>
              </swiper-slide>
              <swiper-slide class="border-r-2 border-base-200">
                <div class="flex flex-col items-center justify-center text-sm">
                  <h3 class="opacity-70">{{ today.dt6 }}</h3>
                  <p class="font-bold">
                    {{ Math.round(weatherAPI.list[6].main.temp) }}&deg;C
                  </p>
                  <img :src="today.src6" alt="temp" class="w-[50px] h-[50px]" />
                  <p
                    class="text-center flex items-center justify-center gap-2 font-semibold"
                  >
                    <i class="fa-duotone fa-windsock"></i
                    >{{ weatherAPI.list[6].wind.speed }} м/с
                  </p>
                </div></swiper-slide
              >
              <swiper-slide>
                <div class="flex flex-col items-center justify-center text-sm">
                  <h3 class="opacity-70">{{ today.dt7 }}</h3>
                  <p class="font-bold">
                    {{ Math.round(weatherAPI.list[7].main.temp) }}&deg;C
                  </p>
                  <img :src="today.src7" alt="temp" class="w-[50px] h-[50px]" />
                  <p
                    class="text-center flex items-center justify-center gap-2 font-semibold"
                  >
                    <i class="fa-duotone fa-windsock"></i
                    >{{ weatherAPI.list[7].wind.speed }} м/с
                  </p>
                </div></swiper-slide
              >
            </swiper>
          </div>
          <div class="flex items-center justify-between gap-4">
            <div
              class="bg-base-300 shadow-md p-2 rounded-md flex flex-col items-center gap-2 w-[90px]"
            >
              <i class="fa-duotone fa-droplet-percent text-3xl lg:text-4xl"></i>
              <p>{{ weatherAPI.list[0].main.humidity }}%</p>
            </div>
            <div
              class="bg-base-300 p-2 shadow-md rounded-md flex flex-col items-center gap-2 w-[90px]"
            >
              <i class="fa-duotone fa-wind text-3xl lg:text-4xl"></i>
              <p>{{ weatherAPI.list[0].wind.speed }} m/s</p>
            </div>
            <div
              class="bg-base-300 p-2 shadow-md rounded-md flex flex-col items-center gap-2 w-[90px]"
            >
              <i
                class="fa-duotone fa-temperature-half text-3xl lg:text-4xl"
              ></i>
              <p>{{ today.tempAverage }}&deg;C</p>
            </div>
          </div>
        </div>
        <div v-else-if="error" class="w-[276px]">
          <h2 class="bg-error text-sm text-slate-200 p-2 rounded-lg">
            {{ error }}
          </h2>
        </div>
      </div>
      <!-- прогноз на 3 дня -->
      <div
        class="self-center lg:self-auto bg-base-200 rounded-md flex flex-col gap-4 shadow-md p-4 w-[340px]"
      >
        <h1 class="text-center lg:p-2 text-2xl font-bold">
          Погода на 3 дня <br />
          {{ cityName }}
        </h1>
        <div class="flex flex-col gap-[52px] items-center" v-if="weatherAPI">
          <div class="flex flex-col items-center">
            <h2 class="flex items-center">
              {{ dataFirstDay.numberDay }}.{{ dataFirstDay.month }},
              {{ dataFirstDay.dayOfWeek }} <span class="px-1">-</span>
              <span class="font-bold"
                >{{ Math.round(firstDay.main.temp) }}&deg;C</span
              >
              <img :src="dataFirstDay.src" alt="cityName" class="w-12 h-12" />
            </h2>
            <div class="flex gap-4">
              <div
                class="bg-base-300 p-2 shadow-md rounded-md flex flex-col items-center gap-2 w-[90px]"
              >
                <i
                  class="fa-duotone fa-droplet-percent text-3xl lg:text-4xl"
                ></i>
                <p>{{ firstDay.main.humidity }}%</p>
              </div>
              <div
                class="bg-base-300 p-2 shadow-md rounded-md flex flex-col items-center gap-2 w-[90px]"
              >
                <i class="fa-duotone fa-wind text-3xl lg:text-4xl"></i>
                <p>{{ firstDay.wind.speed }} m/s</p>
              </div>
              <div
                class="bg-base-300 p-2 shadow-md rounded-md flex flex-col items-center gap-2 w-[90px]"
              >
                <i
                  class="fa-duotone fa-temperature-half text-3xl lg:text-4xl"
                ></i>
                <p>{{ dataFirstDay.tempAverage }}&deg;C</p>
              </div>
            </div>
          </div>
          <div class="flex flex-col items-center">
            <h2 class="flex items-center">
              {{ dataSecondDay.numberDay }}.{{ dataSecondDay.month }},
              {{ dataSecondDay.dayOfWeek }} <span class="px-1">-</span>
              <span class="font-bold">
                {{ Math.round(secondDay.main.temp) }}&deg;C</span
              >
              <img :src="dataSecondDay.src" alt="cityName" class="w-12 h-12" />
            </h2>
            <div class="flex gap-4">
              <div
                class="bg-base-300 p-2 shadow-md rounded-md flex flex-col items-center gap-2 w-[90px]"
              >
                <i
                  class="fa-duotone fa-droplet-percent text-3xl lg:text-4xl"
                ></i>
                <p>{{ secondDay.main.humidity }}%</p>
              </div>
              <div
                class="bg-base-300 p-2 shadow-md rounded-md flex flex-col items-center gap-2 w-[90px]"
              >
                <i class="fa-duotone fa-wind text-3xl lg:text-4xl"></i>
                <p>{{ secondDay.wind.speed }} m/s</p>
              </div>
              <div
                class="bg-base-300 p-2 shadow-md rounded-md flex flex-col items-center gap-2 w-[90px]"
              >
                <i
                  class="fa-duotone fa-temperature-half text-3xl lg:text-4xl"
                ></i>
                <p>{{ dataSecondDay.tempAverage }}&deg;C</p>
              </div>
            </div>
          </div>
          <div class="flex flex-col items-center">
            <h2 class="flex items-center">
              {{ dataThirdDay.numberDay }}.{{ dataThirdDay.month }},
              {{ dataThirdDay.dayOfWeek }} <span class="px-1">-</span>
              <span class="font-bold">
                {{ Math.round(thirdDay.main.temp) }}&deg;C
              </span>
              <img :src="dataThirdDay.src" alt="cityName" class="w-12 h-12" />
            </h2>
            <div class="flex gap-4">
              <div
                class="bg-base-300 p-2 shadow-md rounded-md flex flex-col items-center gap-2 w-[90px]"
              >
                <i
                  class="fa-duotone fa-droplet-percent text-3xl lg:text-4xl"
                ></i>
                <p>{{ thirdDay.main.humidity }}%</p>
              </div>
              <div
                class="bg-base-300 p-2 shadow-md rounded-md flex flex-col items-center gap-2 w-[90px]"
              >
                <i class="fa-duotone fa-wind text-3xl lg:text-4xl"></i>
                <p>{{ thirdDay.wind.speed }} m/s</p>
              </div>
              <div
                class="bg-base-300 p-2 shadow-md rounded-md flex flex-col items-center gap-2 w-[90px]"
              >
                <i
                  class="fa-duotone fa-temperature-half text-3xl lg:text-4xl"
                ></i>
                <p>{{ dataThirdDay.tempAverage }}&deg;С</p>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- График -->
      <div
        class="self-center lg:self-auto bg-base-200 rounded-md flex flex-col items-center gap-4 shadow-md p-4 w-[340px]"
      >
        <h1 class="text-center lg:p-2 lg:pb-6 text-2xl font-bold pb-6">
          Погода в Ростове-на-Дону за прошедшие три дня
        </h1>
        <BarChart v-if="weatherAPI" />
        <!-- SWIPER -->
        <!-- <div class="flex items-center justify-center w-60">
          <swiper class="mySwiper bg-base-300 p-2" :slidesPerView="3">
            <swiper-slide>Slide 1</swiper-slide
            ><swiper-slide>Slide 2</swiper-slide
            ><swiper-slide>Slide 3</swiper-slide
            ><swiper-slide>Slide 4</swiper-slide
            ><swiper-slide>Slide 5</swiper-slide
            ><swiper-slide>Slide 6</swiper-slide
            ><swiper-slide>Slide 7</swiper-slide
            ><swiper-slide>Slide 8</swiper-slide
            ><swiper-slide>Slide 9</swiper-slide>
          </swiper>
        </div> -->
        <!-- <SwiperTest /> -->
      </div>
    </div>
  </div>
</template>
