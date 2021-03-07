
 <template v-slot:top>
    
 <v-card>
 
    <v-card-title>
 <v-spacer></v-spacer>
      <v-text-field
        v-model="search"
        append-icon="mdi-magnify"
        label="Search"
        single-line
        hide-details
      ></v-text-field>

    </v-card-title>

    <!--Modal pour l'ajout d'une enquete -->
      <v-dialog v-model="dialogAdd" persistent max-width="600px">
      <template v-slot:activator="{ on, attrs }">
          <v-btn color="primary" dark v-bind="attrs" v-on="on">
              Nouvvelle Enquete
          </v-btn>
      </template>
      <v-card>
          <v-card-title>
              <span class="headline">Ajouter une enquete</span>
          </v-card-title>
          <v-card-text>
              <v-container>
                  <v-row>
                      <v-col cols="12">
                          <v-text-field label="Name*" v-model="name" required>
                          </v-text-field>
                      </v-col>
                      <v-col cols="12">
                          <v-text-field label="city*" v-model="city" required>
                          </v-text-field>
                      </v-col>
                       <v-col cols="12">
                          <v-text-field label="age*" v-model="age" required>
                          </v-text-field>
                      </v-col>
                      <v-col cols="12">
                          <v-text-field label="voiture*" v-model="voiture" required>
                          </v-text-field>
                      </v-col>
                       <v-col cols="12">
                          <v-text-field label="type*" v-model="type" required>
                          </v-text-field>
                      </v-col>
                      <v-col cols="12">
                          <v-text-field label="marque*" v-model="marque" required>
                          </v-text-field>
                      </v-col>
                  </v-row>
              </v-container>
              <small>*indique qu'il faut remplir le champ</small>
          </v-card-text>
          <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="dialogAdd = false">Close</v-btn>
              <v-btn color="blue darken-1" text @click="saveInvestigations">Save</v-btn>
          </v-card-actions>
      </v-card>
  </v-dialog>

  <!--fin modal d'ajout-->
      <!-- Modal Update Product -->
                  
                        <v-dialog v-model="dialogEdit" persistent max-width="600px">
                            <v-card>
                                <v-card-title>
                                    <span class="headline">Modifier l'enquete</span>
                                </v-card-title>
                                <v-card-text>
                                    <v-container>
                                        <v-row>
                                          <v-col cols="12">
                          <v-text-field label="Name*" v-model="name" required>
                          </v-text-field>
                      </v-col>
                      <v-col cols="12">
                          <v-text-field label="city*" v-model="city" required>
                          </v-text-field>
                      </v-col>
                       <v-col cols="12">
                          <v-text-field label="age*" v-model="age" required>
                          </v-text-field>
                      </v-col>
                      <v-col cols="12">
                          <v-text-field label="voiture*" v-model="voiture" required>
                          </v-text-field>
                      </v-col>
                       <v-col cols="12">
                          <v-text-field label="type*" v-model="type" required>
                          </v-text-field>
                      </v-col>
                      <v-col cols="12">
                          <v-text-field label="marque*" v-model="marque" required>
                          </v-text-field>
                      </v-col>
                                        </v-row>
                                    </v-container>
                                    <small>*indicates required field</small>
                                </v-card-text>
                                <v-card-actions>
                                    <v-spacer></v-spacer>
                                    <v-btn color="blue darken-1" text @click="dialogEdit = false">Close</v-btn>
                                    <v-btn color="blue darken-1" text @click="updateInvestigations">Update</v-btn>
                                </v-card-actions>
                            </v-card>
                        </v-dialog>
                 
 
                    <!-- Modal Delete Product -->
                  
                        <v-dialog v-model="dialogDelete" persistent max-width="600px">
                            <v-card>
                                <v-card-title>
                                    <span class="headline"></span>
                                </v-card-title>
                                <v-card-text>
                                    <v-container>
                                        <v-row>
                                            <h2>Are sure want to delete {{ name }} ?</h2>
                                        </v-row>
                                    </v-container>
                                </v-card-text>
                                <v-card-actions>
                                    <v-spacer></v-spacer>
                                    <v-btn color="blue darken-1" text @click="dialogDelete = false">No</v-btn>
                                    <v-btn color="info darken-1" text @click="dialogDelete= false">Yes
                                    </v-btn>
                                </v-card-actions>
                            </v-card>
                        </v-dialog>
                   
 
    <v-data-table :headers="headers" :items="investigations" :search="search"  class="elevation-1">
      <template v-slot:item.actions="{ item }">
        <v-icon  small class="mr-2" @click="getEdit(item)"> mdi-pencil </v-icon>
        <v-icon small @click="deleteinvestigations(item)" >mdi-delete</v-icon>
      </template>
    </v-data-table>


  </v-card>
</template>


      
 
<script>

import axios from 'axios'
  export default {

    data () {
      return {
        search: '',
   
        headers: [
          {
            text: 'numero',
            align: 'start',
            sortable: false,
            value: 'id',
          },
          { text: 'name', value: 'name' },
          { text: 'city', value: 'city' },
         
          { text: 'Action', value:'actions' }
        ],
        name: '',
        city: '',

        age: '',
        voiture:'',
        type:'',
        marque:'',

          dialogAdd: false,
        dialogEdit: false,
        dialogDelete: false,
        investigationsIdEdit: '',
        investigationsNameEdit: '',
        investigationsPriceEdit: '',
        id: '',
        investigationsNameDelete: '',
        investigations: [],
       
      }
    },
     created: function () {
        this.getInvestigations()
    },
   
      
  

 methods: {    
        // Get investigations
       getInvestigations : function () {
            axios
            .get('http://localhost:3001/investigations/')
            .then((res) => {
                    this.investigations = res.data;
                     console.log(this.investigations);
                });
               
        },



       // Create New investigations
        saveInvestigations: function () {
            axios.post('http://localhost:3001/investigations/', {
          
                   name: this.name,
                    city: this.city,
                    age: this.age,
                    voiture: this.voiture,
                    type: this.type,
                    marque: this.marque
                })
                .then(res => {
                    // handle success
                    this.getInvestigations();
                    this.name = '';
                    this.city = '';
                    this.age = '';
                    this.voiture = '';
                    this.type = '';
                    this.marque = '';
                    this.dialogAdd = false;
                    console.log(res);

                })
                
        },
 
        // Get Edit and Show data to Modal
        getEdit: function (item) {
            this.dialogEdit = true;
            this.id = item.id;
            this.name = item.name;
            this.city = item.city;

            this.age = item.age;
             this.voiture = item.voiture;
            this.type = item.type;
             this.marque = item.marque;
            console.log(this.id);
        },
 
       
 
        // Update investigations
        updateInvestigations: function () {
            axios.put(`http://localhost:3001/investigations/` + this.id, {
                   name: this.name,
                    city: this.city,
                    age: this.age,
                    voiture: this.voiture,
                    type: this.type,
                    marque: this.marque
                })
                .then(res => {
                    // handle success
                    this.getInvestigations();
                    this.dialogEdit = false;
                     console.log(res);

                })
                
        },



//Get Delete and Show Confirm Modal
 getDelete: function (item) {
 this.dialogDelete = true;
 this.id = item.id;
 // this.name = investigations.name;
  }, 
 
        // Delete investigations
        deleteinvestigations: function (item) {

            if(confirm("vous voulez vraiment supprimer cette enqute ?")){
                axios.delete(`http://localhost:3001/investigations/`+item.id)
                .then(res => {
                    // handle success
                    this.getInvestigations();
                    this.dialogDelete = false;

                    console.log(res);
                })

            }
           
          
                
        }
  }


 }
</script>
