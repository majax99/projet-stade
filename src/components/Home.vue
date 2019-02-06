<template>
<div >
	<div >
		<div class= "bandeau" ></div>
  <v-toolbar dark color="#000" >

    <v-toolbar-title class="text-md-center" style="width:100%">
    	<a href="https://www.staderennais.com/stade-emploi">
    		<img class="responsive_logo" src="	https://www.staderennais.com/sites/default/files/logo_0.png">
    	</a>

       <v-spacer  class="spacer font-weight-bold responsive_titre">STADE DE L'EMPLOI 2019</v-spacer>
    </v-toolbar-title>
    

  </v-toolbar>
</div>
<div>
  <v-card style= "margin-top:5%;margin-left:10%;margin-right:10%;margin-bottom:5%" >
    <v-card-title>
      <h2 class="responsive_offres">Les offres de nos exposants</h2>
        <i id="offres" class="space responsive_offres">({{totalItems}} offre<span v-if = "totalItems > 1">s </span> <span v-else>&nbsp;</span>trouvée<span v-if = "totalItems > 1">s </span>)</i>
      <v-spacer	></v-spacer>
      <v-text-field id="search" class="hidden-sm-and-down" v-model="search" append-icon="search" label="Rechercher" single-line hide-details color="#ca180b" v-on:keyup="nb_offres" ></v-text-field>
    </v-card-title>
  <div id="select" >
  	<v-flex xs12 sm7 md5 lg4 d-flex  style="width:100%;">
  	    <v-select @change="nb_offres"  color= "#ca180b" full-width label="Choisissez votre secteur d'activité" 
  	    :items="[
  	    		'Tout secteurs d\'activités',
  				'Accompagnement à l\'emploi',
  				'Agence d\'emploi',
  				'Banque / Assurances',
  				'Commerce / Alimentation / Grande distribution',
  				'Concession automobile',
  				'Ecole / Formation / Conseil',
  				'Groupement d\'employeurs',
  				'Hâbitat / Bâtiment / Construction & TP',
  				'Hébergement et Infogérance d\'applications critiques',
  				'Immobilier',
  				'Maintenance Aeronotique',
  				'Média',
  				'Propreté et Services Associés',
  				'Protection sociale',
  				'Recrutement',
  				'Réseau Electrique',
  				'Restauration',
  				'Santé',
  				'Service publique',
  				'Services  aux entreprises',
  				'Télécommunications spatiales',
  				'Transport et Logistique',
  				'Vente et réparation de véhicules industriels'
  	    ]"
  	      v-model="offresSecteur">
  	    </v-select>
      </v-flex>
  </div>  

    <v-data-table   ref="dom-element" :headers="headers"  :items="filteredItems" :search="search"  :rows-per-page-items="[25,50,100]"   rows-per-page-text="Offres par page"  >
      <template slot="items" slot-scope="props" >

          <td class="text-xs-center hidden-md-and-up"  style="padding-right: 0px;padding-left: 0px;">
          <v-menu offset-y >
            <v-icon slot="activator" color="#ca180b" dark>control_point</v-icon>
            <v-list three-line>
            <v-list-tile-content v-for="(item, index) in props.item" :key="index" style="padding:5px 10px 5px 10px;">
              <v-list-tile-title><strong>{{index}}</strong></v-list-tile-title>
              <v-list-tile-sub-title>{{item}}</v-list-tile-sub-title>
            </v-list-tile-content>
            </v-list>
          </v-menu>
          </td>
          <td class="text-xs-center stand hidden-xs-only" >{{ props.item.Stand }} </td>
          <td class="text-uppercase text-xs-left  " >{{ props.item.Entreprise }}</td>
          <td class="text-xs-left" >{{ props.item.Poste }}</td>
          <td class="text-xs-left hidden-xs-only" >{{ props.item.Localisation }}</td>
          <td class="text-xs-left hidden-sm-and-down " >{{ props.item.Type }}</td>
          <td class="text-xs-center hidden-sm-and-down " >{{ props.item['Nombre de postes'] }}</td>
      </template>
          <template slot="pageText" slot-scope="props">
      Lignes {{ props.pageStart }} - {{ props.pageStop }} de {{ props.itemsLength }}
    </template>
      <v-alert slot="no-results" :value="true" color="error" icon="warning">
        La recherche pour "{{ search }}" n'a pas donnée de résultats
      </v-alert>
    </v-data-table>
  </v-card>

</div>
</div>

</template>

<script>
  import json from '../assets/json/data.json'
  export default {
    data () {
      return {
        search: '',
        totalItems: 0,
        headers: [
          { text: '', value: '' , align: 'center' , class: 'hidden-md-and-up', sortable:false, width:"1%" },
          { text: 'Stand', value: 'Stand' , align: 'center' , class: 'font-weight-black caption hidden-xs-only', sortable:true, width:"5%" },
          { text: 'Entreprise', value: 'Entreprise' , align: 'left', class: 'font-weight-black ', sortable:true , width:"10%"},
          { text: 'Poste', value: 'Poste' , align: 'left' , class: 'font-weight-black', sortable:true , width:"15%"},
          { text: 'Localisation', value: 'Localisation' , align: 'left' , class: 'font-weight-black hidden-xs-only  ', sortable:true , width:"15%"},
          { text: 'Type de contrat', value: 'Type' , align: 'left' , class: 'font-weight-black hidden-sm-and-down', sortable:true , width:"5%"},
          { text: 'Nombre de postes', value: 'Nombre de postes' , align: 'center' , class: 'font-weight-black hidden-sm-and-down', sortable:true , width:"5%"}
          
        ],
        offres: json.offres,
        offresSecteur: null,
        item: {},
        dialog: false,
        dialogm1: ''
        //pagination: {'sortBy': 'Stand', 'descending': false, 'page': 1, 'rowsPerPage': 50}
      }
    },
    mounted() {
    	/*const totalItems = document.querySelector(".v-datatable__actions__pagination").innerHTML;
    	const regex = new RegExp("[0-9]*$");
    	const matches = regex.exec(totalItems)
    	this.totalItems = matches[0] | 0;*/
    	this.totalItems = this.$refs['dom-element'].itemsLength;
  	},
  	methods: {
  		nb_offres: function() {
  		  //console.log(this.$refs['dom-element'].itemsLength);
  		  this.totalItems = this.$refs['dom-element'].itemsLength;
  		}
  	},
    computed: {
      filteredItems() {
        return this.offres.filter((i) => {
      	  if (this.offresSecteur === 'Tout secteurs d\'activités') {
      	  this.offresSecteur = null;
          }
          return !this.offresSecteur || (i.Secteur === this.offresSecteur);
        })
      }
  	},
  	updated() {
  		//console.log(this.$refs['dom-element'].itemsLength);
  		this.totalItems = this.$refs['dom-element'].itemsLength;
  	}

  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
.bandeau {
	width: 100%;
	height: 90px;
	background-color: #ca180b;
}
.spacer {
	font-size: 30px;
	font-family: 'Open Sans",sans-serif';
}
#search {
	width: 5%;
}
.space {
	margin-left: 1%;
}
.responsive_titre {
  width:100%;
}

.responsive_logo {
  position:absolute;
  top:-70px;
  left:10%;
}

table.v-table tbody tr td {
  font-size: 12px;
}



@media screen and (max-width: 800px){
.responsive_logo {
  width:30px;
  top:8px;
  left:10%;
}
.responsive_titre {
  font-size:16px;
  width:100%;
  z-index:-1 ;
}
.responsive_offres {
  font-size:16px;
  width:100%;
}
.stand {
  width:60%;
}
.bandeau {
  width: 100%;
  height: 56px;
  background-color: #ca180b;
}
thead tr th {
  font-size: 10px;
}
table.v-table tbody tr td {
  font-size: 9px;
}


}

@media screen and (max-width: 400px){
.responsive_logo {
  width:25px;
  top:15px;
  left:5%;
}

.responsive_titre {
  font-size:16px;
  width:100%;
  z-index:-1 ;
}
.responsive_offres {
  font-size:16px;
  width:100%;
}
.stand {
  width:60%;
}
v-card {
  font-size:0.8em;
}

}


</style>
