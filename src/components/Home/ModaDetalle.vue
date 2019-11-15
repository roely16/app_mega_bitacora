<template>
    <div class="text-center">
        <v-dialog
            v-model="showModal" 
            light          
        >
      <v-card>
        
        <div class="text-center" v-if="isLoading">
            <!-- <v-progress-circular :size="50" color="primary" indeterminate></v-progress-circular> -->

            <v-sheet
                :color="`lighten-4`"
                class="mx-auto pt-12 pb-12 text-center"
                max-width="300"
                height="763"
                style="position: relative;"
            >
                <v-progress-circular
                :size="70"
                :width="7"
                color="primary"
                indeterminate
                style="margin: 0;
  position: absolute;
  top: 50%;
  -ms-transform: translateY(-50%);
  transform: translateY(-50%);"
                ></v-progress-circular>
            </v-sheet>
        </div>

        <div v-if="!isLoading">
            <!-- <v-card-title>
         Detalle de Matrícula {{ matricula }}
        </v-card-title> -->

        <v-container>
            <v-row >
                <!-- <v-col cols="3">
                    <v-text-field label="Matrícula" outlined readonly></v-text-field>
                </v-col>
                <v-col cols="3">
                    <v-text-field label="Fecha de Inscripción" outlined readonly></v-text-field>
                </v-col>
                <v-col cols="2">
                    <v-text-field label="Finca" outlined readonly></v-text-field>
                </v-col>
                <v-col cols="2">
                    <v-text-field label="Folio" outlined readonly></v-text-field>
                </v-col>
                <v-col cols="2">
                    <v-text-field label="Libro" outlined readonly></v-text-field>
                </v-col> -->
                <v-col cols="6">
                    <v-row>
                        <v-col cols="12">
                            <v-text-field label="Matrícula" dense v-model="detalle_matricula.MATRICULA"  readonly></v-text-field>
                        </v-col>
                        <v-col cols="12">
                            <v-text-field label="Dirección del Inmueble" dense  readonly></v-text-field>
                        </v-col>
                        <v-col cols="12">
                            <v-text-field label="Titular" dense v-model="detalle_matricula.PROPIETARIO" readonly></v-text-field>
                        </v-col>
                        <v-col cols="12">
                            <v-text-field label="Domicilio" dense readonly></v-text-field>
                        </v-col>
                    </v-row>
                </v-col>
                <v-col cols="6">
                    <v-row>
                        <v-col cols="12">
                            <v-text-field label="No. Registral" dense v-model="detalle_matricula.NO_REGISTRAL" readonly></v-text-field>
                        </v-col>
                        <v-col cols="12">
                            <v-text-field label="No. Catastral" dense v-model="detalle_matricula.NO_CATASTRAL" readonly></v-text-field>
                        </v-col>
                    </v-row>
                </v-col>
            </v-row>

            <v-row>
                <v-col>
                    <v-data-table
                        :headers="headers"
                        :items="items"
                        :items-per-page="5"
                        hide-default-footer
                        :page.sync="page"
                    >
                        <template v-slot:top>
                            <v-toolbar dense flat>
                                
                                <v-row>
                                    <v-col cols="8">
                                        <v-toolbar-title>Histórico de Transacciones</v-toolbar-title>
                                    </v-col>
                                    <v-col cols="4">
                                        <v-text-field
                                            label="Búsqueda"
                                            placeholder="Ingrese el texto a buscar..."
                                            append-outer-icon="mdi-magnify"
                                            single-line hide-details
                                            clearable
                                            dense
                                        ></v-text-field>
                                    </v-col>
                                   
                                </v-row>
                            </v-toolbar>
                        </template>
                    </v-data-table>
                </v-col>
            </v-row>

            <div class="text-center">
                <v-pagination dark :total-visible="10" v-model="page" :length="Math.round(items.length / 10) + 1"></v-pagination>
            </div>
        </v-container>
        </div>
        

      </v-card>
    </v-dialog>
  </div>
</template>

<script>

    import axios from 'axios'

    export default {
        props: {
            matricula: {
                type: String,
                default: null,
                required: false,
                
            }
        },
        data(){
            return {
                isLoading: false,
                showModal: false,
                page: 1,
                pageCount: 10,
                items: [

                ],
                headers: [],
                desserts: [],
                detalle_matricula: {}
            }
        },
        methods: {
            obtenerDetalle(){

                console.log('obtener detalle')

                this.isLoading = !this.isLoading

                let data = {
					name: "obtenerDetalle",
					param: {
						matricula: this.matricula,
					}
                }
                
                axios
				.post(process.env.VUE_APP_API_URL, data)
				.then(response => {
					console.log(response.data)
                    this.items = response.data.response.result.items_bitacora
                    this.headers = response.data.response.result.headers_bitacora
                    this.detalle_matricula = response.data.response.result.detalle_matricula
                    this.isLoading = !this.isLoading
				})

            }
        },
        watch: {
            matricula: function(val){

                if (val) {
                    this.showModal = true
                    this.obtenerDetalle()
                }

            }
        }
        // computed: {
        //     show: function(){

        //         return this.showSheet

        //     }
        // }
    }
</script>