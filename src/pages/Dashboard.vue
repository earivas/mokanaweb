<template>
  <div class="content">
    <div class="md-layout">
      <!-- TARJETAS DE INFORMACION -->

      <div class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-25">
        <stats-card data-background-color="green">
          <template slot="header">
            <md-icon>monetization_on</md-icon>
          </template>

          <template slot="content">
            <p class="category">Ventas</p>
            <h3 class="title">
              ${{ ventaActual_ML}}
              <small>M</small>
            </h3>
          </template>

          <template slot="footer">
            <div class="stats">
              <md-icon v-if="crec_porc<0" class="text-danger">thumb_down</md-icon>
              <md-icon v-if="crec_porc>0" class="text-success">thumb_up</md-icon>
              Crec: ${{ crec_vlr }} || {{ crec_porc }}%
              <a href="#pablo">&nbsp; Ver...</a>
            </div>
          </template>
        </stats-card>
      </div>
      <div class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-25">
        <stats-card data-background-color="orange">
          <template slot="header">
            <md-icon>bar_chart</md-icon>
          </template>

          <template slot="content">
            <p class="category">Presupuesto</p>
            <h3 class="title">
              ${{ presupuestoML}}
              <small>M</small>
            </h3>
          </template>

          <template slot="footer">
            <div class="stats">
              <!--
              <md-icon class="text-danger">dehaze</md-icon>
               -9999% y 69%   : Rojo
                  70% y 84%   : Amarillo
                  85% y 99%   : Verde
                 100% y 1000% : Azul
                   <p v-if="number > 5 && number <10">{{number}} esta entre 5 y 10</p>
                     <span v-if="cumpl_porc<69" class="text-danger">
                  <i class="fas fa-long-arrow-alt-down"></i>
              </span>
              <span v-if="cumpl_porc>70 && cumpl_porc< 84" class="text-warning">
                  <i class="fas fa-long-arrow-alt-up"></i>
              </span>
              <span v-if="cumpl_porc>85 && cumpl_porc< 99" class="text-success">
                  <i class="fas fa-long-arrow-alt-up"></i>
              </span>
                    <span v-if="cumpl_porc>100" class="text-info">
                  <i class="fas fa-long-arrow-alt-up"></i>
              </span>
              -->

              <md-icon v-if="cumpl_porc<69" class="text-danger">cancel</md-icon>
              <md-icon v-if="cumpl_porc>70 && cumpl_porc< 84" class="text-warning">warning</md-icon>
              <md-icon v-if="cumpl_porc>85 && cumpl_porc< 99" class="text-success">info</md-icon>
              <md-icon v-if="cumpl_porc>100" class="text-info">check_circle</md-icon>
              Cumpl: ${{ cumpl_vlr}} || {{ cumpl_porc }}%
              <a href="#/presupuesto">&nbsp; Ver ...</a>
            </div>
          </template>
        </stats-card>
      </div>
      <div class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-25">
        <stats-card data-background-color="red">
          <template slot="header">
            <md-icon>business_center</md-icon>
          </template>

          <template slot="content">
            <p class="category">Recaudo</p>
            <h3 class="title">
              ${{ vlr_ppto_recaudo }}
              <small>M</small>
            </h3>
          </template>

          <template slot="footer">
            <div class="stats">
              <md-icon v-if="cump_rec_porc<69" class="text-danger">cancel</md-icon>
              <md-icon v-if="cump_rec_porc>70 && cump_rec_porc< 84" class="text-warning">warning</md-icon>
              <md-icon v-if="cump_rec_porc>85 && cump_rec_porc< 99" class="text-success">info</md-icon>
              <md-icon v-if="cump_rec_porc>100" class="text-info">check_circle</md-icon>
              Cump: ${{ cump_rec_valor }} || {{cump_rec_porc}}%
              <a href="#/cartera">&nbsp; Ver ...</a>
            </div>
          </template>
        </stats-card>
      </div>
      <div class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-25">
        <stats-card data-background-color="blue">
          <template slot="header">
            <!-- <i class="fab fa-twitter"></i> -->
            <md-icon>pie_chart</md-icon>
          </template>

          <template slot="content">
            <p class="category">Margen</p>
            <h3 class="title">{{ margen_porc }}%</h3>
          </template>

          <template slot="footer">
            <div class="stats">
              <md-icon>local_atm</md-icon>Margen del ejecutivo
            </div>
          </template>
        </stats-card>
      </div>

      <!-- FIN TARJETAS DE INFORMACION -->

      <!--   GRAFICAS DE INFORMACION -->

      <!--  VENTAS -->
      <!--  Inicio reubicar grafico charjs -->

      <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-33">
        <v-card>
          <p>Mostar etiquetas</p>
          <v-layout>
            <v-flex>
              <canvas id="myChart"></canvas>

              <v-card-title>
                <h4 class="title">Ventas Mensuales</h4>
                <p class="category">
                  <span class="text-success">
                    <i class="fas fa-long-arrow-alt-down"></i> 55%
                  </span>
                  increase in today sales.
                </p>
              </v-card-title>
            </v-flex>
          </v-layout>
        </v-card>
      </div>

      <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-33">
        <v-card>
          <p>Mostar etiquetas</p>
          <v-layout>
            <v-flex>
              <canvas id="myChartVtaPpto"></canvas>

              <v-card-title>
                <h4 class="title">Comparativo Ventas</h4>
                <p class="category">
                  <span class="text-success">
                    <i class="fas fa-long-arrow-alt-down"></i> 55%
                  </span>
                  increase in today sales.
                </p>
              </v-card-title>
            </v-flex>
          </v-layout>
        </v-card>
      </div>
      <!--  Fin reubicar grafico charjs -->
      <!--  <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-33">
      
        <v-card >
           <template>
            <canvas id="myChart">
           </canvas>
         
          <template slot="content">
            <h4 class="title">Ventas Mensuales xxx</h4>
            <p class="category">
              <span class="text-success">
                <i class="fas fa-long-arrow-alt-down"></i> 55%
              </span>
              increase in today sales.
            </p>
          </template>
          <template slot="footer">
            <div class="stats">
              <md-icon>access_time</md-icon>updated 4 minutes ago
            </div>
          </template>
           </template>
        </v-card>
        
      </div>-->

      <!-- <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-33">
        <chart-card
          :chart-data="dailySalesChart.data"
          :chart-options="dailySalesChart.options"
          :chart-type="'Line'"
          data-background-color="blue"
        >
          <template slot="content">
            <h4 class="title">Ventas Mensuales</h4>
            <p class="category">
              <span class="text-success">
                <i class="fas fa-long-arrow-alt-down"></i> 55%
              </span>
              increase in today sales.
            </p>
          </template>

          <template slot="footer">
            <div class="stats">
              <md-icon>access_time</md-icon>updated 4 minutes ago
            </div>
          </template>
        </chart-card>
      </div>-->

      <!--  PRESUPUESTOS -->
      <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-33">
        <chart-card
          :chart-data="emailsSubscriptionChart.data"
          :chart-options="emailsSubscriptionChart.options"
          :chart-responsive-options="emailsSubscriptionChart.responsiveOptions"
          :chart-type="'Bar'"
          data-background-color="red"
        >
          <template slot="content">
            <h4 class="title">Presupuesto Mes</h4>
            <p class="category">Last Campaign Performance</p>
          </template>

          <template slot="footer">
            <div class="stats">
              <md-icon>access_time</md-icon>updated 10 days ago
            </div>
          </template>
        </chart-card>
      </div>
      <!--   PEDIDOS -->
      <!-- <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-33">
        <chart-card
          :chart-data="dataCompletedTasksChart.data"
          :chart-options="dataCompletedTasksChart.options"
          :chart-type="'Line'"
          data-background-color="green"
        >
          <template slot="content">
            <h4 class="title">Pedidos Mes</h4>
            <p class="category">Last Campaign Performance</p>
          </template>

          <template slot="footer">
            <div class="stats">
              <md-icon>access_time</md-icon>campaign sent 26 minutes ago
            </div>
          </template>
        </chart-card>
      </div>-->
      <!-- FIN  GRAFICAS DE INFORMACION -->

      <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-50">
        <md-card>
          <md-card-header data-background-color="orange">
            <h4 class="title">Últimas facturas</h4>
            <p class="category">Facturas de los últimos 30 días</p>
          </md-card-header>
          <md-card-content>
            <ordered-table table-header-color="orange"></ordered-table>
          </md-card-content>
        </md-card>
      </div>
      <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-50">
        <nav-tabs-card>
          <template slot="content">
            <span class="md-nav-tabs-title">Top 10:</span>
            <md-tabs md-sync-route class="md-success" md-alignment="left">
              <md-tab id="tab-home" md-label="Productos" md-icon="local_grocery_store">
                <nav-tabs-table></nav-tabs-table>
              </md-tab>

              <md-tab id="tab-pages" md-label="Clientes" md-icon="supervisor_account">
                <nav-tabs-table></nav-tabs-table>
              </md-tab>

              <!-- <md-tab id="tab-posts" md-label="server" md-icon="cloud">
                <nav-tabs-table></nav-tabs-table>
              </md-tab>-->
            </md-tabs>
          </template>
        </nav-tabs-card>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Chart from "chart.js";
import {
  StatsCard,
  ChartCard,
  NavTabsCard,
  NavTabsTable,
  OrderedTable
} from "@/components";
import { parse } from "path";

export default {
  components: {
    StatsCard,
    ChartCard,
    NavTabsCard,
    NavTabsTable,
    OrderedTable
  },
  data() {
    return {
      indicadores: [],
      recaudo: [],
      ventaActual_ML: 0,
      ventaAnterior_ML: 0,
      presupuestoML: 0,
      crec_porc: 0,
      cumpl_porc: 0,
      crec_vlr: 0,
      cumpl_vlr: 0,
      margen_porc: 0,
      vlr_recuado: 0,
      vlr_ppto_recaudo: 0,
      cump_rec_porc: 0,
      cump_rec_valor: 0,

      nombreMeses: [],
      totalMeses: [],
      mesesValores: null,

      mesesValoresAnt: null,
      nombreMesesAnt: [],
      totalMesesAnt: [],

      mesesValoresPpto: null,
      nombreMesesPpto: [],
      totalMesesPpto: [],

      monthSalesChart: {
        nombreMes: [],
        totalMeses: [],
        valoresMeses: null,
        data: {
          labels: ["M", "T", "W", "T", "F", "S", "S"],
          series: [[12, 17, 7, 17, 23, 18, 38]]
        },
        options: {
          lineSmooth: this.$Chartist.Interpolation.cardinal({
            tension: 0
          }),
          low: 0,
          high: 50, // creative tim: we recommend you to set the high sa the biggest value + something for a better look
          chartPadding: {
            top: 0,
            right: 0,
            bottom: 0,
            left: 0
          }
        }
      },

      dailySalesChart: {
        data: {
          labels: ["M", "T", "W", "T", "F", "S", "S"],
          series: [[12, 17, 7, 17, 23, 18, 38]]
        },

        options: {
          lineSmooth: this.$Chartist.Interpolation.cardinal({
            tension: 0
          }),
          low: 0,
          high: 50, // creative tim: we recommend you to set the high sa the biggest value + something for a better look
          chartPadding: {
            top: 0,
            right: 0,
            bottom: 0,
            left: 0
          }
        }
      },
      dataCompletedTasksChart: {
        data: {
          labels: ["12am", "3pm", "6pm", "9pm", "12pm", "3am", "6am", "9am"],
          series: [[230, 750, 450, 300, 280, 240, 200, 190]]
        },

        options: {
          lineSmooth: this.$Chartist.Interpolation.cardinal({
            tension: 0
          }),
          low: 0,
          high: 1000, // creative tim: we recommend you to set the high sa the biggest value + something for a better look
          chartPadding: {
            top: 0,
            right: 0,
            bottom: 0,
            left: 0
          }
        }
      },
      emailsSubscriptionChart: {
        data: {
          labels: [
            "Ja",
            "Fe",
            "Ma",
            "Ap",
            "Mai",
            "Ju",
            "Jul",
            "Au",
            "Se",
            "Oc",
            "No",
            "De"
          ],
          series: [[542, 443, 320, 780, 553, 453, 326, 434, 568, 610, 756, 895]]
        },
        options: {
          axisX: {
            showGrid: false
          },
          low: 0,
          high: 1000,
          chartPadding: {
            top: 0,
            right: 5,
            bottom: 0,
            left: 0
          }
        },
        responsiveOptions: [
          [
            "screen and (max-width: 640px)",
            {
              seriesBarDistance: 5,
              axisX: {
                labelInterpolationFnc: function(value) {
                  return value[0];
                }
              }
            }
          ]
        ]
      }
    };
  },
  created() {
    this.ind_vtappto(), this.ind_recaudo();
    // this.chartMonth()
  },
  methods: {
    activar() {
      let me = this;
      axios
        .put("api/BI_PoolVendedores/Activar/" + this.adId, {})
        .then(function(response) {
          me.ventaActual_ML = 0;
          me.ventaActual_ML = 0;
          me.presupuestoML = "";
          //    me.adId="";
          me.listar();
        })
        .catch(function(error) {
          console.log(error);
        });
    },

    getchartVentasMeses() {
      let me = this;
      axios
        .get("api/BI_PRESUPUESTOS/VentasMensualizadas")
        .then(response => {
          console.log("Mostrar datos chart", response);
          me.valoresMeses = response.data;
          me.loadChartVentasMeses();
          me.loadChartVentasPpto();
        })
        .catch(function(error) {
          console.log(error);
        });
    },

    loadChartVentasMeses() {
      let me = this;
      let mesTexto = "";
      me.valoresMeses.map(function(x) {
        switch (parseInt(x.etiqueta)) {
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
        me.totalMeses.push(x.valorActual.toFixed(1));
      });
      var ctx = document.getElementById("myChart");
      ctx.style.backgroundColor = "rgba(0, 167, 86, 1)";
      var myChart = new Chart(ctx, {
        type: "bar",
        data: {
          labels: me.nombreMeses,
          datasets: [
            {
              //  label: 'Ventas Mesuales',
              data: me.totalMeses,
              backgroundColor: [
                "rgba(224, 246, 235, 1)",
                "rgba(224, 246, 235, 1)",
                "rgba(224, 246, 235, 1)",
                "rgba(224, 246, 235, 1)",
                "rgba(224, 246, 235, 1)",
                "rgba(224, 246, 235, 1)",
                "rgba(224, 246, 235, 1)",
                "rgba(224, 246, 235, 1)",
                "rgba(224, 246, 235, 1)",
                "rgba(224, 246, 235, 1)",
                "rgba(224, 246, 235, 1)",
                "rgba(224, 246, 235, 1)"
              ],
              borderColor: ["rgba(224, 246, 235, 1)"],
              borderWidth: 0
            }
          ]
        },
        options: {
          title: {
            display: true
          },

          legend: {
            display: false
          },
          scales: {
            yAxes: [
              {
                ticks: {
                  beginAtZero: true,
                  fontColor: "rgba(224, 246, 235, 1)",
                  fontSize: 9
                },

                gridLines: {
                  color: "rgba(224, 246, 235, 0.1)",
                  borderDash: [8, 1],
                  offsetGridLines: true
                }
              }
            ],
            xAxes: [
              {
                ticks: {
                  beginAtZero: true,
                  fontColor: "rgba(224, 246, 235, 1)",
                  fontSize: 9
                },
                gridLines: {
                  color: "rgba(224, 246, 235, 0.1)",
                  borderDash: [8, 1],
                  offsetGridLines: true
                }
              }
            ]
          },
          responsive: true
        }
      });
    },
    
    getchartVentasPpto() {
      let me = this;
      axios
        .get("api/BI_PRESUPUESTOS/VentasMensualizadas")
        .then(response => {
          console.log("Mostrar datos chart", response);
          me.valoresMeses = response.data;
          me.loadChartVentasPpto();
        })
        .catch(function(error) {
          console.log(error);
        });
    },

    loadChartVentasPpto() {
      let me = this;
      let mesTexto = "";
      me.valoresMeses.map(function(x) {
        switch (parseInt(x.etiqueta)) {
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
        me.nombreMesesPpto.push(mesTexto);
        me.totalMeses.push(x.valorActual.toFixed(1));
        me.totalMesesAnt.push(x.valorAnterior);
        me.totalMesesPpto.push(x.valorPresupuesto.toFixed(1));
      });
      var ctx = document.getElementById("myChartVtaPpto");
      ctx.style.backgroundColor = "rgba(4, 191, 191, 1)";
      var myChart = new Chart(ctx, {
        type: "bar",
        data: {
          labels: me.nombreMesesPpto,
          datasets: [
            {
              //  label: 'Ventas Mesuales',
              data: me.totalMeses,
              backgroundColor: [
                "rgba(224, 246, 235, 1)",
                "rgba(224, 246, 235, 1)",
                "rgba(224, 246, 235, 1)",
                "rgba(224, 246, 235, 1)",
                "rgba(224, 246, 235, 1)",
                "rgba(224, 246, 235, 1)",
                "rgba(224, 246, 235, 1)",
                "rgba(224, 246, 235, 1)",
                "rgba(224, 246, 235, 1)",
                "rgba(224, 246, 235, 1)",
                "rgba(224, 246, 235, 1)",
                "rgba(224, 246, 235, 1)"
              ],
              borderColor: ["rgba(224, 246, 235, 1)"],
              borderWidth: 0
            },
            {
              data: me.totalMesesPpto,
              backgroundColor: [
                "rgba(20, 89, 166, 0.6)",
                "rgba(20, 89, 166, 0.6)",
                "rgba(20, 89, 166, 0.6)",
                "rgba(20, 89, 166, 0.6)",
                "rgba(20, 89, 166, 0.6)",
                "rgba(20, 89, 166, 0.6)",
                "rgba(20, 89, 166, 0.6)",
                "rgba(20, 89, 166, 0.6)",
                "rgba(20, 89, 166, 0.6)",
                "rgba(20, 89, 166, 0.6)",
                "rgba(20, 89, 166, 0.6)",
                "rgba(20, 89, 166, 0.6)"
              ]
            },
            {
              data: me.totalMesesAnt,
              type: "line"
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
            yAxes: [
              {
                ticks: {
                  beginAtZero: true,
                  fontColor: "rgba(224, 246, 235, 1)",
                  fontSize: 9
                },

                gridLines: {
                  color: "rgba(224, 246, 235, 0.1)",
                  borderDash: [8, 1],
                  offsetGridLines: true
                }
              }
            ],
            xAxes: [
              {
                ticks: {
                  beginAtZero: true,
                  fontColor: "rgba(224, 246, 235, 1)",
                  fontSize: 9
                },
                gridLines: {
                  color: "rgba(224, 246, 235, 0.1)",
                  borderDash: [8, 1],
                  offsetGridLines: true
                }
              }
            ]
          },
          responsive: true
        }
      });
    },
    ind_vtappto() {
      let me = this;
      axios
        .get("api/BI_PRESUPUESTOS/VentasPresupuestos")
        .then(function(response) {
          console.log("Metodo Listar Indicadores ", response);
          me.indicadores = response.data;
          me.ventaActual_ML = response.data[0].ventaActual_ML.toFixed(1);
          me.ventaAnterior_ML = response.data[0].ventaAnterior_ML.toFixed(1);
          me.presupuestoML = response.data[0].presupuestoML.toFixed(2);
          // **** Calculos que debo pasar al Controlador
          me.crec_porc = (
            (response.data[0].ventaActual_ML /
              response.data[0].ventaAnterior_ML -
              1) *
            100
          ).toFixed(2);
          me.cumpl_porc = (
            (response.data[0].ventaActual_ML / response.data[0].presupuestoML) *
            100
          ).toFixed(2);

          me.crec_vlr = (
            response.data[0].ventaActual_ML - response.data[0].ventaAnterior_ML
          ).toFixed(2);
          me.cumpl_vlr = (
            response.data[0].ventaActual_ML - response.data[0].presupuestoML
          ).toFixed(2);
          me.margen_porc = (
            (1 -
              response.data[0].costoActual_ML /
                response.data[0].ventaActual_ML) *
            100
          ).toFixed(1);
          // *****
        })
        .catch(function(error) {
          console.log(error);
        });
    },

    ind_recaudo() {
      let me = this;
      axios
        .get("api/BI_RECAUDO/RecaudoPresupuestos")
        .then(function(response) {
          console.log("Metodo Listar Indicadores Recaudo ", response);
          me.recaudo = response.data;
          me.vlr_recaudo = response.data[0].valorRecaudo.toFixed(2);
          me.vlr_ppto_recaudo = response.data[0].valorPpto.toFixed(2);
          me.cump_rec_porc = response.data[0].porCumpRecaudo.toFixed(1);
          me.cump_rec_valor = response.data[0].valCumpRecaudo.toFixed(2);
          // **** Calculos que debo pasar al Controlador

          // me.cump_rec_valor= ((response.data[0].valorRecaudo - response.data[0].valorPpto)).toFixed(2),
          // me.cump_rec_porc = ((response.data[0].valorRecaudo/response.data[0].valorPpto)*100).toFixed(2)

          // *****
        })
        .catch(function(error) {
          console.log(error);
        });
    }
  },
  mounted() {
    this.getchartVentasMeses();
  }
};
</script>
