<template>
  <div class="container">
    <Bar
      v-if="loaded"
      :chart-data="chartData"
      :chart-options="{ responsive: true, maintainAspectRatio: false }"
      :css-classes="cssClasses"
    />
  </div>
</template>
<!-- http://api.weatherapi.com/v1/history.json?key=68a6fc39ac4b44dc8f3213823221606&dt=2022-06-18&end_dt=2022-06-19&q=London&aqi=yes& -->
<script>
import { ref } from "vue";
import { Bar } from "vue-chartjs";
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale,
} from "chart.js";
// const weatherAPI = ref(null);
// function testAPI() {
//   fetch(
//     "http://apiweatherapi..com/v1/history.json?key=68a6fc39ac4b44dc8f3213823221606&dt=2022-06-18&end_dt=2022-06-19&q=London&aqi=yes&"
//   )
//     .then((response) => response.json())
//     .then((result) => {
//       if (result.cod === "404" || result.cod === "400") {
//         error.value = result.message;
//       } else {
//         weatherAPI.value = result.location.name;
//         console.log(weatherAPI.value);
//       }
//     });
// }
// testAPI();
ChartJS.register(
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale
);

const weatherAPI = ref(null);
const today = ref(0);
const firstDay = ref(0);
const secondDay = ref(0);
const thirdDay = ref(0);
let dataTime = new Date();
const testData = ref({});
// const testOptions = ref({})

function getLastWeatherAPI() {
  fetch(
    `https://api.weatherapi.com/v1/history.json?key=68a6fc39ac4b44dc8f3213823221606&dt=2022-06-${
      dataTime.getDate() - 3
    }&end_dt=2022-06-${dataTime.getDate()}&q=Rostov-On-Don"&aqi=yes&`
  )
    .then((response) => response.json())
    .then((result) => {
      weatherAPI.value = result;
      thirdDay.value = weatherAPI.value.forecast.forecastday[0].day.avgtemp_c;
      secondDay.value = weatherAPI.value.forecast.forecastday[1].day.avgtemp_c;
      firstDay.value = weatherAPI.value.forecast.forecastday[2].day.avgtemp_c;
      today.value = weatherAPI.value.forecast.forecastday[3].day.avgtemp_c;
      console.log(weatherAPI.value.forecast.forecastday[0].day.avgtemp_c);
      testData.value = {
        labels: [
          `${dataTime.getDate() - 3}.0${dataTime.getMonth() + 1}`,
          `${dataTime.getDate() - 2}.0${dataTime.getMonth() + 1}`,
          `${dataTime.getDate() - 1}.0${dataTime.getMonth() + 1}`,
          `${dataTime.getDate()}.0${dataTime.getMonth() + 1}`,
        ],
        datasets: [
          {
            label: "Средняя температура",
            fill: false,
            backgroundColor: "#10b981",
            data: [
              `${thirdDay.value}`,
              secondDay.value,
              firstDay.value,
              today.value,
            ],
            borderWidth: 2,
          },
        ],
      };
    });
}
getLastWeatherAPI();

export default {
  name: "BarChart",
  components: { Bar },
  props: {
    cssClasses: {
      default: "bg-base-300 rounded-md shadow-md p-4",
      type: String,
    },
  },
  data: () => ({
    loaded: true,
    chartData: testData,
    chartOptions: {
      responsive: true,
    },
  }),
  // async mounted() {
  //   this.loaded = false;

  //   try {
  //     await fetch(
  //       'http://api.weatherapi.com/v1/history.json?key=68a6fc39ac4b44dc8f3213823221606&dt=2022-06-18&end_dt=2022-06-19&q=Rostov-On-Don"&aqi=yes&'
  //     )
  //       .then((response) => response.json())
  //       .then((result) => {
  //         console.log(weatherAPI.value);
  //       });

  //     // this.chartdata = testData;
  //     this.loaded = true;
  //   } catch (e) {
  //     console.error(e);
  //   }
  // },
};
</script>
