<template>
  <div>
    <md-table v-model="facturas" :table-header-color="tableHeaderColor">
      <md-table-row slot="md-table-row" slot-scope="{ item }">
        <md-table-cell  md-label="Cliente">{{ item.nombreCliente}}</md-table-cell>
        <md-table-cell class="caption" md-label="Fecha">{{ item.fechaFact | subStr }}</md-table-cell>
        <md-table-cell class="caption" md-label="Factura">{{ item.idFactura }}</md-table-cell>
        <md-table-cell class="caption" md-label="Valor">{{ item.valorTotal }}</md-table-cell>
        <!--  <md-table-cell md-label="City">{{ item.city }}</md-table-cell> -->
      </md-table-row>
    </md-table>
  </div>
</template>

<script>
import axios from "axios";
import VueFilterDateFormat from "vue-filter-date-format";
export default {
  name: "ordered-table",
  props: {
    tableHeaderColor: {
      type: String,
      default: ""
    }
  },
  data() {
    return {
      dateFormatConfig: {
        monthNames: [
          "January",
          "February",
          "March",
          "April",
          "May",
          "June",
          "July",
          "August",
          "September",
          "October",
          "November",
          "December"
        ],
        monthNamesShort: [
          "Jan",
          "Feb",
          "Mar",
          "Apr",
          "May",
          "Jun",
          "Jul",
          "Aug",
          "Sep",
          "Oct",
          "Nov",
          "Dec"
        ]
      },

      facturas: [],
      selected: [],

      users: [
        {
          id: 1,
          name: "Dakota Rice",
          salary: "$36,738",
          country: "Niger",
          city: "Oud-Turnhout"
        },
        {
          id: 2,
          name: "Minerva Hooper",
          salary: "$23,738",
          country: "Curaçao",
          city: "Sinaai-Waas"
        },
        {
          id: 3,
          name: "Sage Rodriguez",
          salary: "$56,142",
          country: "Netherlands",
          city: "Overland Park"
        },
        {
          id: 4,
          name: "Philip Chaney",
          salary: "$38,735",
          country: "Korea, South",
          city: "Gloucester"
        }
      ]
    };
  },
  filters: {
    subStr: function(string) {
      
      return string.substring(0,10);
    }
  },
  created() {
    // this.initialize()
    this.listar();
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
    }
  }
};
</script>
