<template>
  <v-container>
  </br></br></br></br>



    <v-layout row>
      <v-flex xs12 sm6 offset-sm3>
        <h1 class="text--secondary mb-3">Создать новую запись</h1>
        <v-form v-model="valid" ref="form" validation class="mb-3">
          <v-text-field
            name="title"
            label="Добавить заголовок"
            type="text"
            v-model="title"
            required
            :rules="[v => !!v || 'Обязательно к заполнению']"
          ></v-text-field>
          <v-text-field
            name="description"
            label="Добавить описание"
            type="text"
            v-model="description"
            multi-line
            :rules="[v => !!v || 'Обязательно к заполнению']"
          ></v-text-field>
		  
		  
		  
        </v-form>
        <v-layout row class="mb-3">
          <v-flex xs12>
		     <v-btn
               color="#ccc"
			 @click="triggerUpload">
             Загрузить картинку
			</v-btn>
			
		  
		  
            
            <input
              ref="fileInput"
              type="file"
              style="display: none;"
              accept="image/*"
              @change="onFileChange"
            >
			
          </v-flex>
        </v-layout>
        <v-layout row>
          <v-flex xs12>
            <img :src="imageSrc" height="100" v-if="imageSrc">
          </v-flex>
        </v-layout>
        
        <v-layout row>
          <v-flex xs12>
            <v-spacer></v-spacer>
            <v-btn
               color="blue"

              class="success"
              @click="createAd"
            >
			
              Создать
            </v-btn>
          </v-flex>
        </v-layout>
		
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import axios from 'axios';
import keys from '../../keys/keys.js';

  export default {
    data () {
      return {
        title: '',
        description: '',
        valid: false,
        image: null,
        imageSrc: '',
		id:this.$route.params.id,
		article:null,
		img:null,
		keys:keys
      }
    },
    computed: {
      loading () {
      }
    },
    methods: {
      createAd () {
  var fd = new FormData();
  fd.append('title', this.title);
  fd.append('description', this.description);
  fd.append('image', this.image);
  
        axios({
			   method: 'post', 
               url: keys.URI+'/api/store',
			   data: fd,
			     headers: {
                    'Content-Type': 'multipart/form-data'
                }
			  

		 })
			 .then(response => {
				 console.log(response.data)
				 this.$router.push('/articles')

              }).catch(e=>{
					console.log(e);
				})
				
        
      },
      triggerUpload () {
        this.$refs.fileInput.click()
      },
      onFileChange (event) {
        const file = event.target.files[0]


        this.image = file
      }
    },
	
  }
</script>
