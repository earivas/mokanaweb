<template>
  <v-container grid-list-md wrap>
    <v-layout wrap>
      <v-layout wrap>
        <v-flex class="secondary" xs12 sm12 md12 lg12>
          <h4
            class="caption white--text"
          >TITULO INFORME GESTIÓN ECC FORECASTING LINA LOZANO A MAYO 2019</h4>
        </v-flex>

        <!-- Recuadro1-->
        <v-flex class="fondografico" xs12 sm6 md6 lg6>
          <v-card>
            <v-img src="http://i67.tinypic.com/10oij5k.jpg"></v-img>
          </v-card>
        </v-flex>
        <!-- Recuadro2-->
        <v-flex class="fondografico" xs12 sm6 md6 lg6>
          <v-card>
            <v-img src="http://i67.tinypic.com/10oij5k.jpg"></v-img>
          </v-card>
        </v-flex>
      </v-layout>

      <!-- Primer grupo 2 graficas-->

      <v-layout wrap class="fondografico">
        <!-- Segundo Layout Precisión del forecast-->
        <v-flex xs12 class="secondary">
          <h4 class="caption white--text">PRECISIÓN DEL FORECAST POR PRODUCTO</h4>
        </v-flex>
        <v-spacer></v-spacer>

        <v-flex xs12 sm6 md6>
          <v-card mt-1>
            <v-flex>
              <canvas id="myChartForecastP2"></canvas>

              <v-card-title>
                <h4 class="title">OTIFLEX LIMPIADOR * 100 mL</h4>
                <p class="category">
                  <span class="text-success">
                    <i class="fas fa-long-arrow-alt-down"></i> 55%
                  </span>
                  increase.
                </p>
              </v-card-title>
              <!---->
              
              <!---->
        
            </v-flex>
          </v-card>
        </v-flex>
        <v-flex xs12 sm6 md6>
          <v-card>
            <v-flex>
              <canvas id="myChartForecast"></canvas>

              <v-card-title>
                <h4 class="title">TEARS FRASCO X 8ML</h4>
                <p class="category">
                  <span class="text-success">
                    <i class="fas fa-long-arrow-alt-down"></i> 55%
                  </span>
                  increase.
                </p>
              </v-card-title>
            </v-flex>
          </v-card>
        </v-flex>
        <v-flex xs12 sm6 md6>
          <v-card>
            <v-flex>
              <canvas id="myChartForecastP3"></canvas>

              <v-card-title>
                <h4 class="title">ACIFLUX SUSPENSION * 50 ml</h4>
                <p class="category">
                  <span class="text-success">
                    <i class="fas fa-long-arrow-alt-down"></i> 55%
                  </span>
                  increase in today sales.
                </p>
              </v-card-title>
            </v-flex>
            
          </v-card>
        </v-flex>
        <v-flex xs12 sm6 md6>
          <v-card>
            <h1>GRAFICA PRODUCTO 4</h1>
            
          </v-card>
        </v-flex>
      </v-layout>
<!-- Dialogos para graficas modales-->
<div class="text-xs-center">
    <v-dialog
      v-model="dialog3"
      width="500"
    >
      <template v-slot:activator="{ on }">
        <v-btn
          color="red lighten-2"
          dark
          v-on="on"
        >
          Click Me
        </v-btn>
      </template>

      <v-card>
        <v-card-title
          class="headline grey lighten-2"
          primary-title
        >
          Privacy Policy
        </v-card-title>

        <v-card-text>
          Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            flat
            @click="dialog = false"
          >
            I accept
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
<!---->



      <v-layout wrap>
        <!-- Primer grupo 2 graficas-->
        <v-flex class="fondografico" xs12 sm12 md6>
          <h1>TEXTO/TABLA PRODUCTOS CON MEJOR PRECISIÓN</h1>
        </v-flex>
        <v-flex class="fondografico" xs12 sm12 md6>
          <h1>TABLA PRESICIÓN PRODUCTOS PARETO</h1>
        </v-flex>
      </v-layout>
    </v-layout>
  </v-container>
</template>

<script>
Chart.defaults.global.elements.line.fill = false;
import axios from "axios";
import Chart from "chart.js";
//import ChartDataLabels from 'chartjs-plugin-datalabels';

export default {
  data() {
    return {
      grpForecast: [],
      nombreMeses: [],
      totalMesesForecast: [],
      totalMesesVentas: [],
      totalMesesPrecision: [],
      mesesValores: null,
      dialog1: false,
      dialog2: false,
      dialog3: false,
      nombreMesesP2: [],
      totalMesesForecastP2: [],
      totalMesesVentasP2: [],
      totalMesesPrecisionP2: [],
      mesesValoresP2: null,

      nombreMesesP3: [],
      totalMesesForecastP3: [],
      totalMesesVentasP3: [],
      totalMesesPrecisionP3: [],
      mesesValoresP3: null
    };
  },
  created() {
    // this.initialize()
    this.listar();
    this.getchartForecast();
    this.getchartForecastP2();
    this.getchartForecastP3();
  },
  methods: {
    listar() {
      let me = this;
      axios
        .get("api/BI_FACTURAS/ListarUltimasFacturas")
        .then(function(response) {
          console.log("Metodo Listar Facturas", response);
          me.facturas = response.data;
        })
        .catch(function(error) {
          console.log(error);
        });
    },
    getchartForecast() {
      let me = this;
      axios
        .get("api/BI_FORECAST/ListarForecast")
        .then(response => {
          console.log("Mostrar datos chart Forecast", response);
          me.valoresMeses = response.data;
          me.loadChartForecast();
        })
        .catch(function(error) {
          console.log(error);
        });
    },

    loadChartForecast() {
      let me = this;
      let mesTexto = "";
      me.valoresMeses.map(function(x) {
        switch (x.numeroMes) {
          case 1:
            mesTexto = "Ene";

            break;
          case 2:
            mesTexto = "Feb";
            break;
          case 3:
            mesTexto = "Mar";
            break;
          case 4:
            mesTexto = "Abr";
            break;
          case 5:
            mesTexto = "May";
            break;
          case 6:
            mesTexto = "Jun";
            break;
          case 7:
            mesTexto = "Jul";
            break;
          case 8:
            mesTexto = "Ago";
            break;
          case 9:
            mesTexto = "Sep";
            break;
          case 10:
            mesTexto = "Oct";
            break;
          case 11:
            mesTexto = "Nov";
            break;
          case 12:
            mesTexto = "Dic";
            break;
          default:
            mesTexto = "Err";
        }
        me.nombreMeses.push(mesTexto);
        me.totalMesesVentas.push(x.venta.toFixed(1));
        me.totalMesesForecast.push(x.forecast);
        me.totalMesesPrecision.push(x.precision.toFixed(1));
      });
      var ctx = document.getElementById("myChartForecast");
      //    ctx.style.backgroundColor = "rgba(4, 191, 191, 1)";
      var myChart = new Chart(ctx, {
        type: "bar",
        data: {
          labels: me.nombreMeses,
          datasets: [
            {
              //  label: 'Ventas Mesuales',
              data: me.totalMesesForecast,
              type: "bar",
              yAxisID: "y-axis-1",
              backgroundColor: [
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)"
              ],
              borderColor: ["rgba(53, 72, 74, 1)"],
              borderWidth: 0
            },
            {
              data: me.totalMesesVentas,
              type: "bar",
              yAxisID: "y-axis-1",
              backgroundColor: [
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)"
              ]
            },
            {
              data: me.totalMesesPrecision,
              type: "line",
              backgroundColor: ["rgba(245, 55, 35, 1)"],
              borderColor: ["rgba(245, 55, 35, 1)"]
              //id: "y-axis-1",
              //  yAxisID: 'y-axis-2'
            }
          ] // fin dataset
        },
        options: {
          title: {
            display: true
          },
          pointBackgrounColor: false,
          legend: {
            display: false
          },
          scales: {
            xAxes: [
              {
                stacked: false
              }
            ],
            yAxes: [
              {
                stacked: false,
                position: "left",
                id: "y-axis-0"
              },
              {
                stacked: false,
                position: "right",
                id: "y-axis-1"
              }
            ]
            //rgba(53, 72, 74, 1) Verde Oscuro (Forecast)
            // rgba(183, 217, 132, 1) Verde claro  (Venta)
            // Rojo (Precision)
          },
          responsive: true
        }
      });
    },

    getchartForecastP2() {
      let me = this;
      axios
        .get("api/BI_FORECAST/ListarForecastP2")
        .then(response => {
          console.log("Mostrar datos chart ForecastP2", response);
          me.valoresMesesP2 = response.data;
          me.loadChartForecastP2();
        })
        .catch(function(error) {
          console.log(error);
        });
    },

    loadChartForecastP2() {
      let me = this;
      let mesTextoP2 = "";
      me.valoresMesesP2.map(function(x) {
        switch (x.numeroMes) {
          case 1:
            mesTextoP2 = "Ene";

            break;
          case 2:
            mesTextoP2 = "Feb";
            break;
          case 3:
            mesTextoP2 = "Mar";
            break;
          case 4:
            mesTextoP2 = "Abr";
            break;
          case 5:
            mesTextoP2 = "May";
            break;
          case 6:
            mesTextoP2 = "Jun";
            break;
          case 7:
            mesTextoP2 = "Jul";
            break;
          case 8:
            mesTextoP2 = "Ago";
            break;
          case 9:
            mesTextoP2 = "Sep";
            break;
          case 10:
            mesTextoP2 = "Oct";
            break;
          case 11:
            mesTextoP2 = "Nov";
            break;
          case 12:
            mesTextoP2 = "Dic";
            break;
          default:
            mesTextoP2 = "Err";
        }
        me.nombreMesesP2.push(mesTextoP2);
        me.totalMesesVentasP2.push(x.venta.toFixed(1));
        me.totalMesesForecastP2.push(x.forecast);
        me.totalMesesPrecisionP2.push(x.precision.toFixed(1));
      });
      var ctx = document.getElementById("myChartForecastP2");
      //    ctx.style.backgroundColor = "rgba(4, 191, 191, 1)";
      var myChart = new Chart(ctx, {
        type: "bar",
        data: {
          labels: me.nombreMesesP2,
          datasets: [
            {
              //  label: 'Ventas Mesuales',
              data: me.totalMesesForecastP2,
              type: "bar",
              yAxisID: "y-axis-1",
              backgroundColor: [
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)"
              ],
              borderColor: ["rgba(53, 72, 74, 1)"],
              borderWidth: 0
            },
            {
              data: me.totalMesesVentasP2,
              type: "bar",
              yAxisID: "y-axis-1",
              backgroundColor: [
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)"
              ]
            },
            {
              data: me.totalMesesPrecisionP2,
              type: "line",
              backgroundColor: ["rgba(245, 55, 35, 1)"],
              borderColor: ["rgba(245, 55, 35, 1)"]
              //id: "y-axis-1",
              //  yAxisID: 'y-axis-2'
            }
          ] // fin dataset
        },
        options: {
          title: {
            display: true
          },
          pointBackgrounColor: false,
          legend: {
            display: false
          },
          scales: {
            xAxes: [
              {
                stacked: false
              }
            ],
            yAxes: [
              {
                stacked: false,
                position: "left",
                id: "y-axis-0"
              },
              {
                stacked: false,
                position: "right",
                id: "y-axis-1"
              }
            ]
            //rgba(53, 72, 74, 1) Verde Oscuro (Forecast)
            // rgba(183, 217, 132, 1) Verde claro  (Venta)
            // Rojo (Precision)
          },
          responsive: true
        }
      });
    },

    getchartForecastP3() {
      let me = this;
      axios
        .get("api/BI_FORECAST/ListarForecastP3")
        .then(response => {
          console.log("Mostrar datos chart ForecastP3", response);
          me.valoresMesesP3 = response.data;
          me.loadChartForecastP3();
        })
        .catch(function(error) {
          console.log(error);
        });
    },
    loadChartForecastP3() {
      let me = this;
      let mesTextoP3 = "";
      me.valoresMesesP3.map(function(x) {
        switch (x.numeroMes) {
          case 1:
            mesTextoP3 = "Ene";

            break;
          case 2:
            mesTextoP3 = "Feb";
            break;
          case 3:
            mesTextoP3 = "Mar";
            break;
          case 4:
            mesTextoP3 = "Abr";
            break;
          case 5:
            mesTextoP3 = "May";
            break;
          case 6:
            mesTextoP3 = "Jun";
            break;
          case 7:
            mesTextoP3 = "Jul";
            break;
          case 8:
            mesTextoP3 = "Ago";
            break;
          case 9:
            mesTextoP3 = "Sep";
            break;
          case 10:
            mesTextoP3 = "Oct";
            break;
          case 11:
            mesTextoP3 = "Nov";
            break;
          case 12:
            mesTextoP3 = "Dic";
            break;
          default:
            mesTextoP3 = "Err";
        }
        me.nombreMesesP3.push(mesTextoP3);
        me.totalMesesVentasP3.push(x.venta.toFixed(1));
        me.totalMesesForecastP3.push(x.forecast);
        me.totalMesesPrecisionP3.push(x.precision.toFixed(1));
      });
      var ctx = document.getElementById("myChartForecastP3");
      //    ctx.style.backgroundColor = "rgba(4, 191, 191, 1)";
      var myChart = new Chart(ctx, {
        type: "bar",
        data: {
          labels: me.nombreMesesP3,
          datasets: [
            {
              //  label: 'Ventas Mesuales',
              data: me.totalMesesForecastP3,
              type: "bar",
              yAxisID: "y-axis-1",
              backgroundColor: [
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)",
                "rgba(53, 72, 74, 1)"
              ],
              borderColor: ["rgba(53, 72, 74, 1)"],
              borderWidth: 0
            },
            {
              data: me.totalMesesVentasP3,
              type: "bar",
              yAxisID: "y-axis-1",
              backgroundColor: [
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)",
                "rgba(183, 217, 132, 1)"
              ]
            },
            {
              data: me.totalMesesPrecisionP3,
              type: "line",
              backgroundColor: ["rgba(245, 55, 35, 1)"],
              borderColor: ["rgba(245, 55, 35, 1)"]
              //id: "y-axis-1",
              //  yAxisID: 'y-axis-2'
            }
          ] // fin dataset
        },
        options: {
          title: {
            display: true
          },
          pointBackgrounColor: false,
          legend: {
            display: false
          },
          scales: {
            xAxes: [
              {
                stacked: false
              }
            ],
            yAxes: [
              {
                stacked: false,
                position: "left",
                id: "y-axis-0"
              },
              {
                stacked: false,
                position: "right",
                id: "y-axis-1"
              }
            ]
            //rgba(53, 72, 74, 1) Verde Oscuro (Forecast)
            // rgba(183, 217, 132, 1) Verde claro  (Venta)
            // Rojo (Precision)
          },
          plugins: {
            datalabels: {
              anchor: "end",
              align: "top",
              formatter: Math.round,
              font: {
                weight: "bold"
              }
            }
          },
          responsive: true
        }
      });
    },
    mounted() {
      this.getchartForecast();
      this.getchartForecastP2();
      this.getchartForecastP3();
    }
  }
};
</script>