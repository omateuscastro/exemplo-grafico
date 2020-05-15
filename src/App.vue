<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">Exemplo Gráfico</div>

      <v-spacer></v-spacer>

      <v-btn @click="fillData()">
        <span class="mr-2">Atualizar</span>
      </v-btn>
    </v-app-bar>

    <v-content fluid>
      <v-row align="center">
        <div class="small">
          <line-chart :chart-data="datacollection" :options="options"></line-chart>
        </div>
      </v-row>

      <v-row align="center">
        <v-col class="text-center" cols="12" sm="4">
          <p class="display-1">Ótimo</p>

          <v-row justify="space-around">
            <v-color-picker v-model="colorOtimo" hide-inputs></v-color-picker>
          </v-row>

          <v-select v-model="simboloOtimo" filled :items="items" class="ma-8" label="Simbolo" dense></v-select>
        </v-col>

        <v-col class="text-center" cols="12" sm="4">
          <p class="display-1">Regular</p>
          <v-row justify="space-around">
            <v-color-picker v-model="colorRegular" hide-inputs></v-color-picker>
          </v-row>

          <v-select
            v-model="simboloRegular"
            filled
            :items="items"
            class="ma-8"
            label="Simbolo"
            dense
          ></v-select>
        </v-col>

        <v-col class="text-center" cols="12" sm="4">
          <p class="display-1">Ruim</p>
          <v-row justify="space-around">
            <v-color-picker v-model="colorRuim" hide-inputs></v-color-picker>
          </v-row>

          <v-select v-model="simboloRuim" filled :items="items" class="ma-8" label="Simbolo" dense></v-select>
        </v-col>
      </v-row>
    </v-content>
  </v-app>
</template>

<script>
import LineChart from "./LineChart.js";

export default {
  name: "App",

  components: {
    LineChart
  },

  data: () => ({
    colorOtimo: "#0013FFFF",
    colorRegular: "#00BE04FF",
    colorRuim: "#FF0000FF",
    simboloOtimo: "triangle",
    simboloRegular: "circle",
    simboloRuim: "rect",
    items: ["rect", "circle", "triangle"],
    datacollection: null,
    options: {
      responsive: true,
      title: {
        display: true,
        text: "Gols do Grêmio em 2019 :"
      },
      legend: {
        display: false
      },
      elements: {
        point: {
          pointStyle: function(context) {
            var index = context.dataIndex;
            var value = context.dataset.data[index];
            return value < 9
              ? context.dataset.simboloRuim
              : value < 13
              ? context.dataset.simboloRegular
              : context.dataset.simboloOtimo;
          },
          backgroundColor: function(context) {
            var index = context.dataIndex;
            var value = context.dataset.data[index];
            return value < 9
              ? context.dataset.colorRuim
              : value < 13
              ? context.dataset.colorRegular
              : context.dataset.colorOtimo;
          }
        }
      }
    }
  }),
  mounted() {
    this.fillData();
  },
  methods: {
    fillData() {
      this.datacollection = {
        labels: [
          "Jan",
          "Fev",
          "Mar",
          "Abr",
          "Mai",
          "Jun",
          "Jul",
          "Ago",
          "Set",
          "Out",
          "Nov",
          "Dez"
        ],
        datasets: [
          {
            label: "Gols no Mês",
            fill: false,
            pointRadius: 10,
            pointHoverRadius: 15,
            data: [12, 11, 13, 9, 12, 2, 7, 14, 17, 14, 7, 7],
            simboloOtimo: this.simboloOtimo,
            simboloRegular: this.simboloRegular,
            simboloRuim: this.simboloRuim,
            colorOtimo: this.colorOtimo,
            colorRegular: this.colorRegular,
            colorRuim: this.colorRuim
          }
        ]
      };
    }
  }
};
</script>


<style>
.small {
  max-width: 320px;
  margin: 20px auto;
}
</style>
