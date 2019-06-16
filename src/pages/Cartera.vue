<template>
  <div class="content">
    <div class="md-layout">
      <div class="md-layout-item">
        <md-card>
          <md-card-header data-background-color="green">
            <h4 class="title">Recaudo de cartera</h4>
            <p class="category">En esta sección puedes ver la gestión de la cartera</p>
          </md-card-header>
          <md-card-content>
            <div class="md-layout">
              <dx-pivot-grid
                :allow-sorting="true"
                :allow-sorting-by-summary="true"
                :allow-filtering="true"
                :height="620"
                :show-borders="true"
                :data-source="dataSource"
                row-header-layout="tree"
              >
                <dx-scrolling mode="virtual"/>
                 <dx-export :enabled="true" file-name="Cartera"/>
              </dx-pivot-grid>
            </div>
          </md-card-content>
        </md-card>
      </div>
    </div>
  </div>
</template>

<script>
import {
  DxPivotGrid,
  DxScrolling,
  DxFieldChooser,
  DxExport
} from "devextreme-vue/pivot-grid";

import { createStore } from "devextreme-aspnet-data-nojquery";

export default {
  components: {
    DxPivotGrid,
     DxExport,
    DxScrolling
  },
  data() {
    return {
      tile: false,
      dataSource: {
        remoteOperations: false,
        store: createStore({
          key: "id",
          loadUrl: "http://localhost:31530/api/BI_RECAUDO/ListarRecaudo"
        }),
        fields: [
          {
            caption: "Cliente",
            dataField: "cliente",
            width: 450,
            expanded: false,
            sortBySummaryField: "valorPpto",
            sortBySummaryPath: [],
            sortOrder: "desc",
            area: "row"
          },
          {
            caption: "Linea",
            dataField: "linea",
            width: 250,
            sortBySummaryField: "valorPpto",
            sortBySummaryPath: [],
            sortOrder: "desc",
            area: "row"
          },
          {
            caption: "Presupuesto",
            dataField: "valorPpto",
            summaryType: "sum",
            format: "currency",
            width: 50,
            area: "data"
          },
          {
            caption: "Recaudo",
            dataField: "valorRecaudo",
            summaryType: "sum",
            format: "currency",
            width: 50,
            area: "data"
          },
          {
            caption: "% Cumpl",
            dataField: "porCumpRecaudo",
            summaryType: "avg",
            format: "percent",
            area: "data"
          },
          {
            dataField: "Id",
            visible: false
          }
        ]
      }
    };
  }
};
</script>