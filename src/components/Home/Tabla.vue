<template>
    <v-container fluid>

		<v-row>
			<v-col cols="4">
				
			</v-col>
			<v-col>

			</v-col>
		</v-row>

        <v-data-table
            :headers="headers"
            :items="items"
			item-key="MATRICULA"
            :items-per-page="10"
            class="elevation-3"
			hide-default-footer
			:loading="isLoading" 
			loading-text="Cargando... Espere un momento"
			
        >	
			<template v-slot:item.no_registral="{ item }">
				{{ item.MA_FINCA }} - {{ item.MA_FOLIO }} - {{ item.MA_LIBRO }} - {{ item.MA_TIPO_LIBRO }}
			</template>
			<template v-slot:item.no_catastral="{ item }">
				{{ item.MA_NUMERO_ZONA }} - {{ item.MA_NUMERO_MANZANA }} - {{ item.MA_NUMERO_PREDIO }}
			</template>
			<template v-slot:top>
				<!-- <v-toolbar flat>
					<v-row>
						<v-col cols="8">
							<v-toolbar-title>Matrículas</v-toolbar-title>
						</v-col>
						<v-col cols="4">
							<v-text-field
								label="Búsqueda"
								placeholder="Ingrese el número de matrícula"
								append-outer-icon="mdi-magnify"
								@click:append-outer="searchMatricula"
								clearable
							></v-text-field>
						</v-col>
					</v-row>
					
				</v-toolbar> -->
					<v-card-title>
						<v-row align="center">
							<v-col cols="8">
								<strong><strong>Matrículas</strong></strong>
							</v-col>
							<v-col>
								<v-text-field label="Buscar por matrícula..." single-line hide-details append-outer-icon="mdi-magnify" @click:append-outer="searchMatricula" clearable></v-text-field>
							</v-col>
						</v-row>
						
						<!-- <v-spacer></v-spacer> -->
						
					</v-card-title>
			</template>
			<template v-slot:expanded-item="{ headers }">
				
			</template>	
			<template v-slot:item.actions="{ item }">
				<!-- <v-chip :color="getColor(item.calories)" dark>{{ item.calories }}</v-chip> -->
				<!-- {{ item }} -->
				<v-icon @click="modalDetalle(item)">mdi-information</v-icon>
			</template>
		</v-data-table>
		<div class="text-center pt-4">
			<v-pagination dark :total-visible="10" v-model="page" :length="pageCount" @input="obtenerMatriculas"></v-pagination>
		</div>

		<ModalDetalle :matricula="matricula"/>
    </v-container>
</template>

<script>
	
	import axios from 'axios'
	import ModalDetalle from './ModaDetalle'

    export default {
		components: {
			ModalDetalle
		},
        data () {
			return {
				headers: [],
				items: [],
				page: 1,
				pageCount: 100,
				isLoading: false,
				expanded: [],
				singleExpand: false,
				showSheet: false,
				matricula: ''
			}
		},
		methods: {

			obtenerMatriculas(){

				this.isLoading = !this.isLoading

				let data = {
					name: "obtenerMatriculas",
					param: {
						pagina: this.page,
						items_pagina: 10
					}
				}

				axios
				.post(process.env.VUE_APP_API_URL, data)
				.then(response => {
					console.log(response.data)
					this.items = response.data.response.result.items
					this.headers = response.data.response.result.headers
					this.pageCount = response.data.response.result.total_registros
					this.isLoading = !this.isLoading
				})

			},
			searchMatricula(){

			},
			cambiarPagina(){

				console.log('cambiar pagina')

			},
			modalDetalle(item){

				console.log(item)
				this.matricula = item.MATRICULA
			}

		},
		mounted(){
			
			this.obtenerMatriculas()

		}
    }
</script>