<template>
    <div>
        <v-card class="mx-auto main" max-width="344">
            <v-img
              height="200px"
              :src="currentImg"
              cover
            ></v-img>
        
            <v-card-title class="text-center">
              {{ num }} - {{ upperCase }}
            </v-card-title>        
            <v-card-subtitle class="text-center">
                {{ pokemon.type }}
            </v-card-subtitle><br>
            <v-card-content class="text-center d-flex justify-center">
                <v-btn @click="changeSprite" class="btn">
                    change
                  </v-btn>
            </v-card-content>
          </v-card>
    </div>
</template>

<script>
    import axios from 'axios'

    export default{
        created: function(){
            axios.get(this.url).then(res => {
                this.pokemon.type = res.data.types[0].type.name
                this.pokemon.front = res.data.sprites.front_default
                this.pokemon.back = res.data.sprites.back_default
                this.currentImg = this.pokemon.front
            })
        },
        data(){
            return {
                isFront: true,
                currentImg: '',
                pokemon : {
                    type: '',
                    front: '',
                    back: ''
                }
            }
        },
        name: 'PokemonCard',
        props: ['num', 'name', 'url'],
        computed: {
            upperCase: function(){
                var firstLetter = this.name[0].toUpperCase() + this.name.slice(1)               
                return firstLetter
            }
        },
        methods: {
            changeSprite: function(){
                if(this.isFront){
                    this.isFront = false
                    this.currentImg = this.pokemon.back
                }else{
                    this.isFront = true
                    this.currentImg = this.pokemon.front
                }
            }
        }
    }
</script>

<style scoped>
    .btn {
        background-color: lightgray;
        width: 50px;
        height: 25px;
        font-size: 12px;
        border-radius: 3px;
    }

</style>