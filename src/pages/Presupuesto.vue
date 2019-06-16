<template>
  <div class="content">
    <div class="md-layout">
      <div class="md-layout-item">
        <md-card>
          <md-card-header data-background-color="green">
            <h4 class="title">Comparativo de ventas vs presupuestos</h4>
            <p
              class="category"
            >En esta sección puedes comparar ver el cumplimiento del presupuesto de ventas</p>
          </md-card-header>
          <md-card-content>
            <div class="md-layout">
              <div class="md-layout-item md-large-size-100">
                <!--
                <dx-chart ref="chart">
                  <dx-tooltip :enabled="true" :customize-tooltip="customizeTooltip"/>
                  <dx-adaptive-layout :width="450" />
                  <dx-size :height="200"/>
                  <dx-common-series-settings type="bar"/>
                </dx-chart>
                -->

                <!---->
              </div>
            </div>
          </md-card-content>
        </md-card>
      </div>
      <!-- -->
    </div>
    <div>
      <dx-pivot-grid
        ref="grid"
        :data-source="dataSource"
        :allow-sorting-by-summary="true"
        :allow-filtering="true"
        :show-borders="true"
        :show-column-grand-totals="false"
        :show-row-grand-totals="true"
        :show-row-totals="false"
        :show-column-totals="false"
      >
        <dx-export :enabled="true" file-name="Presupuesto"/>
        <dx-field-chooser :enabled="true" :height="400"/>

        <dx-header-filter
          :allow-search="allowSearch"
          :show-relevant-values="showRelevantValues"
          :width="300"
          :height="400"
        />
        <dx-field-panel :visible="true"/>
        <!--  <dx-scrolling mode="virtual"/> -->
      </dx-pivot-grid>
    </div>
  </div>
</template>
<script>
import axios from "axios";
import {
  DxChart,
  DxAdaptiveLayout,
  DxCommonSeriesSettings,
  DxSize,
  DxTooltip
} from "devextreme-vue/chart";

import {
  DxPivotGrid,
  DxFieldChooser,
  DxExport,
  DxScrolling,
  DxHeaderFilter,
  DxFieldPanel
} from "devextreme-vue/pivot-grid";

import { DxCheckBox } from "devextreme-vue/check-box";

//import { sales } from './data.js';
//import { sales2 } from './ppto.js';
import { createStore } from "devextreme-aspnet-data-nojquery";
import "devextreme/dist/css/dx.common.css";
import "devextreme/dist/css/dx.light.compact.css";
import Axios from "axios";

const currencyFormatter = new Intl.NumberFormat("en-US", {
  style: "currency",
  currency: "USD",
  minimumFractionDigits: 0
});

export default {
  components: {
    DxChart,
    DxAdaptiveLayout,
    DxCommonSeriesSettings,
    DxSize,
    DxTooltip,
    DxPivotGrid,
    DxFieldChooser,
    DxExport,
    DxScrolling,
    DxHeaderFilter,
    DxFieldPanel,
    DxCheckBox
  },
  data() {
    return {
      //   expand: false,
      remoteOperations:false,
      allowSearch: true,
      allowCrossGroupCalculation: true,

      showRelevantValues: true,
      scrolling: {
        mode: "virtual"
      },
      dataSource: {
        remoteOperations: false,

        store: createStore({
          key: "id",
          loadUrl:
            "http://localhost:31530/api/BI_PRESUPUESTOS/ListarPresupuesto"
        }),

        fields: [
          {
            caption: "Producto",
            width: 300,
            dataField: "producto",
            area: "row",
            visible: true,
            sortBySummaryField: "ventaMLActual",
            sortBySummaryPath: [],
            sortOrder: "desc"
          },
          {
            caption: "Und.Negocio",
            width: 120,
            dataField: "undNegocio",
            area: "row",
            visible: false,
            sortBySummaryField: "ventaMLActual"
          },
           {
            caption: "Vendedor",
            width: 120,
            dataField: "vendedor",
             area: "filter",
            visible: true,
            sortBySummaryField: "ventaMLActual"
          },
          {
            caption: "Linea",
            dataField: "linea",
            width: 150,
            visible: false,
            area: "row"
          },
          {
            dataField: "fecha",
            dataType: "date",
            area: "column",
            visible: true,
            expanded: false,
         //   filterValues:[['fecha].[quarter].[month]&[May']]
          },
          {
            dataField: "mes",
            dataType: "number",
            area: "filter",
            visible: true,
            expanded: false,
           // filterValues:['[mes].&[5]']
          },

          {
            groupName: "month",
            groupInterval: "month",
            visible: false,
            
          },
          {
            caption: "Año Anterior",
            dataField: "ventaMLAnterior",
            dataType: "number",
            summaryType: "sum",
            format: "currency",
            area: "data"
          },
          {
            caption: "Año Actual",
            dataField: "ventaMLActual",
            dataType: "number",
            summaryType: "sum",
            format: "currency",
            area: "data"
          },
          {
             caption: "Crec. %",
            dataField: "porCrecVenta",
            dataType: "number",
            summaryType: "sum",
            format: "percent",
            area: "data"
          },

          
          {
            caption: "Presupuesto",
            dataField: "pptoMlocal",
            dataType: "number",
            summaryType: "sum",
            format: "currency",
            area: "data"
          },
          {
            caption: "Cumpl $",
            dataField: "valCumpPpto",
            dataType: "number",
            summaryType: "sum",
            format: "currency",
            area: "data"
          },
          {
            caption: "Cumpl %",
            dataField: "porCumpPpto",
            dataType: "number",
            summaryType: "avg",
            format: "percent",
            area: "data"
          },
          {
            dataField: "nombreG1",
            area: "filter"
          },

          {
            dataField: "id",
            visible: false
          }
        ]
        //  store: sales
      },
      customizeTooltip: function(args) {
        const valueText = currencyFormatter.format(args.originalValue);
        return {
          html: `${
            args.seriesName
          } | Total<div class='currency'>${valueText}</div>`
        };
      }
    };
  },
  mounted() {
    const pivotGrid = this.$refs.grid.instance;
    //* const chart = this.$refs.chart.instance;
    //* pivotGrid.bindChart(chart, {
    //*   dataFieldsDisplayMode: "splitPanes",
    //*   alternateDataFields: false
    //*  });
    const dataSource = pivotGrid.getDataSource();
    setTimeout(function() {
      dataSource.expandHeaderItem("row", ["Avícola"]);
      dataSource.expandHeaderItem("column", [2019]);
    }, 0);
  }
};
</script>
<style>
.dx-pivotgrid {
  margin-top: 20px;
}
.currency {
  text-align: center;
}

#sales {
  max-height: 570px;
}

.options {
  padding: 20px;
  margin-top: 20px;
  background-color: rgba(191, 191, 191, 0.15);
}

.caption {
  font-size: 18px;
  font-weight: 500;
}

.option {
  width: 100%;
  display: inline-block;
  margin-top: 10px;
}
</style>