<template>
  <div class="pb-5">
    <div >
      <v-toolbar dark color="#ca180b"  class="bandeau" >
        <v-spacer  >STADE DE L'EMPLOI 2019</v-spacer>  
      </v-toolbar>

    <v-toolbar dark color="#000" class="typo_DIN" >
      <v-toolbar-title class="text-md-center" style="width:100%">
        <a href="https://www.staderennais.com/stade-emploi">
          <img class="responsive_logo" src="  https://www.staderennais.com/sites/default/files/logo_0.png">
        </a>
         <v-spacer  class="spacer  responsive_titre">28 Février 2019<v-icon  color="#ca180b" dark class="arrow text-xs-center">arrow_forward</v-icon>Roazhon Park</v-spacer>
         <v-spacer  class="spacer  responsive_titre">Ouverture de 9h00 à 17h30<v-icon  color="#ca180b" dark class="arrow text-xs-center ">arrow_forward</v-icon>Accès visiteurs porte 15</v-spacer>
       </v-toolbar-title>
      </v-toolbar>
    </div>
    <div class="typo_DIN">
      <v-card       :class="{'mx-0 mt-0': $vuetify.breakpoint.smAndDown, 'mx-5 mt-5': $vuetify.breakpoint.mdAndUp}" class="mb-4"  >
        <v-card-title>
          <h2 class="responsive_offres">Les offres de nos exposants</h2>
            <span id="offres" class="ml-2 responsive_offres">({{totalItems}} offre<span v-if = "totalItems > 1">s </span> <span v-else>&nbsp;</span>trouvée<span v-if = "totalItems > 1">s </span>)</span>
          <v-spacer ></v-spacer>
          <v-text-field id="search" class="hidden-sm-and-down" v-model="search" append-icon="search" label="Rechercher" single-line hide-details color="#ca180b" v-on:keyup="nb_offres" ></v-text-field>
        </v-card-title>
        <div id="select" >
          <v-flex xs12 sm5 md3 lg3 d-flex  style="width:100%;">
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
              'Industrie Agroalimentaire',
              'Industrie systèmes logistiques automatisés',
              'Maintenance Aéronautique',
              'Média',
              'Propreté et Services Associés',
              'Protection sociale',
              'Recrutement',
              'Réseau Electrique',
              'Restauration',
              'Santé',
              'Service public',
              'Services  aux entreprises',
              'Télécommunications spatiales',
              'Transport et Logistique',
              'Vente et Réparation de Véhicules Industriels',
              'AUTRES'
            ]"
              v-model="offresSecteur">
            </v-select>
          </v-flex>
        </div>  

        <v-data-table   ref="dom-element" :headers="headers"  :items="filteredItems" :search="search"  :rows-per-page-items="[25,50,100]"   rows-per-page-text="Offres par page" class="elevation-1"   >
          <template slot="items" slot-scope="props" >
              <td class="text-xs-center hidden-md-and-up px-0">
                <v-menu offset-y >
                  <v-icon slot="activator" color="#ca180b" dark>control_point</v-icon>
                  <v-list three-line>
                  <v-list-tile-content v-for="(item, index) in props.item" :key="index"  class="px-3">
                    <v-list-tile-title><strong>{{index}}</strong></v-list-tile-title>
                    <v-list-tile-sub-title>{{item}}</v-list-tile-sub-title>
                  </v-list-tile-content>
                  </v-list>
                </v-menu>
              </td>
              <td class="text-xs-center  hidden-xs-only " >{{ props.item['Numéro du stand'] }} </td>
              <td class="text-uppercase text-xs-left" >{{ props.item.Entreprise }}</td>
              <td class="text-xs-left mr-2" >{{ props.item['Poste à pourvoir'] }}</td>
              <td class="text-xs-left hidden-xs-only " >{{ props.item.Localisation }}</td>
              <td class="text-uppercase text-xs-left hidden-sm-and-down  " >{{ props.item.Contrat }}</td>
              <td class="text-xs-center hidden-sm-and-down  " >{{ props.item['Nombre de postes'] }}</td>
          </template>
              <template slot="pageText" slot-scope="props">
          Lignes {{ props.pageStart }} - {{ props.pageStop }} sur {{ props.itemsLength }}
        </template>
          <v-alert slot="no-results" :value="true" color="error" icon="warning">
            La recherche pour "{{ search }}" n'a pas donnée de résultats
          </v-alert>
          <v-alert slot="no-data" :value="true" color="error" icon="warning">
            Aucun poste trouvé pour ce secteur.
          </v-alert>
        </v-data-table>
      </v-card>

    </div>
      <v-footer class="pa-5 text-xs-center" color="#fafafa" >
      <v-flex  py-3  text-xs-center text-sm-right xs12 >
        <span  style="color:rgb(0,163,224)" class="mr-2">Réalisé par</span> <br>
        <a href="https://www.neo-soft.fr/" >
          <img src="../../public/neosoft.png" style="width:70px;height:83px;" class="mt-2 ">
        </a>
      </v-flex>
    </v-footer>
  </div>

</template>

<script>
  import json from '../../public/data.json'
  export default {
    data () {
      return {
        search: '',
        totalItems: 0,
        headers: [
          { text: '', value: '' , align: 'center' , class: 'hidden-md-and-up', sortable:false, width:"1%" },
          { text: 'Stand', value: 'Numéro du stand' , align: 'center' , class: ' hidden-xs-only subheading ', sortable:true, width:"5%" },
          { text: 'Entreprise', value: 'Entreprise' , align: 'left', class: ' subheading ', sortable:true , width:"10%"},
          { text: 'Intitulé du poste', value: 'Poste à pourvoir' , align: 'left' , class: ' subheading', sortable:true , width:"20%"},
          { text: 'Localisation', value: 'Localisation' , align: 'left' , class: ' subheading hidden-xs-only ', sortable:true , width:"15%"},
          { text: 'Type de contrat', value: 'Contrat' , align: 'left' , class: ' subheading hidden-sm-and-down', sortable:true , width:"5%"},
          { text: 'Nombre de postes', value: 'Nombre de postes' , align: 'center' , class: 'subheading hidden-sm-and-down', sortable:true , width:"5%"}
          
        ],
        offres: json,
        offresSecteur: null,
        item: {}
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
          return !this.offresSecteur || (i['Secteur d\'activité'] === this.offresSecteur);
        })
      }
    },
    watch: {
      offresSecteur: function () {
          if (this.offresSecteur === 'Tout secteurs d\'activités') {
            this.offresSecteur = null;
          }
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

  @font-face {
  font-family: "Sunrise";
  src: url('../assets/font/Sunrise International.otf');
  }

  @font-face {
  font-family: "DIN";
  src: url('../assets/font/DIN/DIN Condensed Bold.ttf');
  }

  .typo_DIN {
    font-family: "DIN";
  }

  * i{ font-family: 'Material Icons'; }

  .bandeau {
    width: 100%;
    height: 90px;
    background-color: #ca180b;
    font-family: "Sunrise";
    font-size: 50px;
    padding-top:15px;
  }
  #search {
    width: 5%;
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
    font-size: 16px;
  }


  @media screen and (max-width: 960px){
  .responsive_logo {
    width:30px;
    top:8px;
    left:10%;
  }
  .responsive_titre {
    font-size:14px;
    width:100%;
    z-index:-1 ;
  }
  .responsive_offres {
    font-size:16px;
    width:100%;
  }

  .bandeau {
    width: 100%;
    height: 56px;
    background-color: #ca180b;
    padding-top: 0;
    font-size:30px;
  }

  table.v-table tbody tr td {
    font-size:0.8em;
  }


}

  @media screen and (max-width: 400px){
  .responsive_logo {
    width:25px;
    top:15px;
    left:5%;
  }

  .responsive_titre {
    font-size:11px;
    width:100%;
    z-index:-1 ;
  }
  .responsive_offres {
    font-size:16px;
    width:100%;
  }

  .arrow {
  font-size: 15px;
  }

  table.v-table tbody tr td {
    font-size:0.8em;
  }  


}


</style>
