<template>
  <div class="content">
    <div class="md-layout">
      <div class="md-layout-item">
        <md-card>
          <md-card-header data-background-color="green">
            <h4 class="title">Gestión de usuarios</h4>
            <p
              class="category"
            >En esta sección podrá: Crear, actualizar, desactivar y activar usuarios</p>
          </md-card-header>
          <md-card-content>
            <div class="md-layout">
              <div class="md-layout-item md-large-size-100">
                <!---->
                <v-layout aling-start>
                  <v-flex data-app>
                    <v-toolbar flat color="white">
                      <v-toolbar-title>Vendedores</v-toolbar-title>
                      <v-divider class="mx-2" inset vertical></v-divider>
                      <v-spacer></v-spacer>
                      <v-text-field
                        class="text-xs-center"
                        v-model="search"
                        append-icon="search"
                        label="Búsqueda"
                        single-line
                        hide-details
                      ></v-text-field>

                      <v-spacer></v-spacer>

                      <v-dialog v-model="dialog" max-width="500px">
                        <template v-slot:activator="{ on }">
                          <v-btn color="blue" dark class="mb-2" v-on="on">Nuevo</v-btn>
                        </template>
                        <v-card>
                          <v-card-title>
                            <span class="headline">{{ formTitle }}</span>
                          </v-card-title>

                          <v-card-text>
                            <v-container grid-list-md>
                              <v-layout wrap>
                                <v-flex xs12 sm12 md4>
                                  <v-text-field v-model="userDomain" label="Usuario"></v-text-field>
                                </v-flex>
                                <v-flex xs12 sm6 md4>
                                  <v-text-field v-model="idVendedor" label="Codigo"></v-text-field>
                                </v-flex>
                                <v-flex xs12 sm6 md4>
                                  <v-text-field v-model="email" label="Email"></v-text-field>
                                </v-flex>
                                <v-flex xs12 sm6 md4>
                                  <v-select
                                    :items="['1000','1055','1065','3010','5000','8000','9000']"
                                    label="Sociedad*"
                                    required
                                  ></v-select>
                                  <!-- <v-text-field v-model="idOrgVentas" label="Sociedad"></v-text-field> -->
                                </v-flex>
                                <v-flex xs12 sm6 md4>
                                  <v-text-field v-model="vendFilter" label="Codigo Padre"></v-text-field>
                                </v-flex>
                              </v-layout>
                            </v-container>
                          </v-card-text>

                          <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn color="blue darken-1" flat @click="close">Cancelar</v-btn>
                            <v-btn color="blue darken-1" flat @click="guardar">Guardar</v-btn>
                          </v-card-actions>
                        </v-card>
                      </v-dialog>
                      <!--dialogo Activar / Desactivar-->
                      <v-dialog v-model="adModal" max-width="290" max-height="300">
                        <v-card>
                          <v-card-title class="headline" v-if="adAccion==1">Activar Vendedor?</v-card-title>
                          <v-card-title class="headline" v-if="adAccion==2">Desactivar Vendedor?</v-card-title>
                          <v-card-text>
                            Realmente desea
                            <span v-if="adAccion==1">Activar</span>
                            <span v-if="adAccion==2">Desactivar</span>
                            el Vendedor {{aduserDomain}}
                          </v-card-text>
                          <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn
                              color="green darken-1"
                              flat="flat"
                              @click="activarDesactivarCerrar"
                            >Cancelar</v-btn>
                            <v-btn
                              v-if="adAccion==1"
                              color="orange darken-4"
                              flat="flat"
                              @click="activar"
                            >Activar</v-btn>
                            <v-btn
                              v-if="adAccion==2"
                              color="orange darken-4"
                              flat="flat"
                              @click="desactivar"
                            >Desactivar</v-btn>
                          </v-card-actions>
                        </v-card>
                      </v-dialog>
                    </v-toolbar>

                    <v-data-table
                      :headers="headers"
                      :items="usuarios"
                      :search="search"
                      :loading="true"
                      :pagination.sync="pagination"
                      class="elevation-1"
                    >
                      <v-progress-linear v-slot:progress color="blue" indeterminate></v-progress-linear>
                      <template v-slot:items="props">
                        <td class="justify-center layout px-0">
                          <v-icon small class="mr-2" @click="editItem(props.item)">edit</v-icon>
                          <template v-if="props.item.estado">
                            <v-icon small @click="activarDesactivarMostrar(2,props.item)">block</v-icon>
                          </template>

                          <template v-else>
                            <v-icon small @click="activarDesactivarMostrar(1,props.item)">check</v-icon>
                          </template>
                        </td>
                        <td>{{ props.item.userDomain }}</td>
                        <td>{{ props.item.idVendedor }}</td>
                        <td>{{ props.item.email }}</td>
                        <td>{{ props.item.idOrgVentas }}</td>
                        <td>{{ props.item.vendFilter }}</td>
                        <td>
                          <div v-if=" props.item.estado == true">
                            <v-badge left color="green">
                              <template v-slot:badge>
                                <span>A</span>
                              </template>
                            </v-badge>
                          </div>
                          <div v-else>
                            <v-badge left color="red">
                              <template v-slot:badge>
                                <span>I</span>
                              </template>
                            </v-badge>
                          </div>
                        </td>
                      </template>
                      <template v-slot:no-data>
                        <v-btn color="primary" @click="listar">No se encontraron registros : Resetear</v-btn>
                      </template>
                    </v-data-table>
                    <!--   <div class="text-xs-center pt-2">
        <v-pagination v-model="pagination.page" :length="pages"></v-pagination>
      </div>
                    -->
                  </v-flex>
                </v-layout>
                <!---->
              </div>
            </div>
          </md-card-content>
        </md-card>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { log } from "util";
export default {
  data() {
    return {
      pagination: {},
      usuarios: [],
      valida: 0,
      reg: /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,24}))$/,
      validamensaje: [],
      usr: [],
      dialog: false,
      selected: [],

      headers: [
        { text: "Acciones", value: "name", sortable: false },
        { text: "Usuario", value: "userDomain" },
        { text: "Vendedor", value: "idVendedor" },
        { text: "Email", value: "email" },
        { text: "Empresa", value: "idOrgVentas" },
        { text: "Vendedor Padre", value: "vendFilter" },
        { text: "Estado", value: "estado" }
      ],
      adModal: 0,
      adAccion: 0,
      aduserDomain: "",
      adId: "",
      search: "",
      usuarios: [],
      editedIndex: -1,
      editedItem: {
        name: "",
        calories: 0,
        fat: 0,
        carbs: 0,
        protein: 0
      },
      id: "",
      userDomain: "",
      idVendedor: "",
      email: "",
      idOrgVentas: "",
      vendFilter: "",
      estado: ""
    };
  },

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "Nuevo Vendedor" : "Actualizar Vendedor";
    },
    pages() {
      if (
        this.pagination.rowsPerPage == null ||
        this.pagination.totalItems == null
      )
        return 0;
      return Math.ceil(
        this.pagination.totalItems / this.pagination.rowsPerPage
      );
    }
  },

  watch: {
    dialog(val) {
      val || this.close();
    }
  },

  created() {
    // this.initialize()
    this.listar(), this.mostrar();
  },
  mounted() {
    //   let xu=this;
    //   var self1 = this;
    //    axios.get('api/BI_PoolVendedores/Listar')
    //          .then(function(res){
    //            console.log('Data mounted', res.data)
    //           self1.usuarios=res.data;
    //          })
    //          .catch(function(error){
    //            console.log('Error: ', error)
    //          })
  },

  methods: {
    listar() {
      let me = this;
      axios
        .get("api/BI_PoolVendedores/Listar")
        .then(function(response) {
          console.log("Metodo Listar ", response);
          me.usuarios = response.data;
        })
        .catch(function(error) {
          console.log(error);
        });
    },

    mostrar() {
      axios.get("api/BI_PoolVendedores/Listar").then(response => {
        console.log("Metodo mostrar ", response);
        this.usr = response.data.data;
      });
    },

    initialize() {},

    editItem(item) {
      this.id = item.idUserDomain;
      this.userDomain = item.userDomain;
      this.idVendedor = item.idVendedor;
      this.email = item.email;
      this.idOrgVentas = item.idOrgVentas;
      this.vendFilter = item.vendFilter;
      this.editedIndex = 1;
      this.dialog = true;
    },

    deleteItem(item) {
      const index = this.desserts.indexOf(item);
      confirm("Are you sure you want to delete this item?") &&
        this.desserts.splice(index, 1);
    },

    close() {
      this.dialog = false;
    },

    limpiar() {
      this.userDomain = "";
      this.idVendedor = "";
      this.email = "";
      this.idOrgVentas = "";
      this.vendFilter = "";
    },

    guardar() {
      if (this.editedIndex > -1) {
        //Codigo para editar
        //codigo para guardar
        let me = this;
        axios
          .put("api/BI_PoolVendedores/Actualizar", {
            idUserDomain: me.id,
            userDomain: me.userDomain,
            idVendedor: me.idVendedor,
            email: me.email,
            idOrgVentas: me.idOrgVentas,
            vendFilter: me.vendFilter
          })
          .then(function(response) {
            me.close();
            me.listar();
            me.limpiar();
          })
          .catch(function(error) {
            console.log(error);
          });
      } else {
        //codigo para guardar
        let me = this;
        axios
          .post("api/BI_PoolVendedores/Crear", {
            userDomain: me.userDomain,
            idVendedor: me.idVendedor,
            email: me.email,
            idOrgVentas: me.idOrgVentas,
            vendFilter: me.vendFilter
          })
          .then(function(response) {
            me.close();
            me.listar();
            me.limpiar();
          })
          .catch(function(error) {
            console.log(error);
          });
      }
      this.close();
    },
    validar() {
      this.valida = 0;
      this.validamensaje = [];

      if ((x = 1)) {
      }
    },

    activarDesactivarMostrar(accion, item) {
      let adMe = this;
      adMe.adModal = 1;
      adMe.aduserDomain = item.userDomain;
      adMe.adId = item.idUserDomain;

      if (accion == 1) {
        adMe.adAccion = 1;
      } else if (accion == 2) {
        adMe.adAccion = 2;
      } else {
        this.adModal = 0;
      }
    },

    activar() {
      let me = this;
      axios
        .put("api/BI_PoolVendedores/Activar/" + this.adId, {})
        .then(function(response) {
          me.adModal = 0;
          me.adAccion = 0;
          me.aduserDomain = "";
          me.adId = "";
          me.listar();
        })
        .catch(function(error) {
          console.log(error);
        });
    },

    desactivar() {
      let me = this;
      axios
        .put("api/BI_PoolVendedores/Desactivar/" + this.adId, {})
        .then(function(response) {
          me.adModal = 0;
          me.adAccion = 0;
          me.aduserDomain = "";
          me.adId = "";
          me.listar();
        })
        .catch(function(error) {
          console.log(error);
        });
    },

    activarDesactivarCerrar() {
      this.adModal = 0;
    }
  }
};
</script>
