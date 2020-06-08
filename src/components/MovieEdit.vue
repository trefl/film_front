<template>
    <div>
        <label for ="title">Tytuł</label><br/>
        <input id="title" placeholder = "title" v-model="localMovie.title"/><br/>
         <label for ="description">Opis</label><br/>
        <textarea id="description" placeholder = "description" v-model="localMovie.description"/><br/>
        <button @click="saveMovie()">Zapisz</button>
    </div>
</template>


<script>
export default {
    name: "MovieEdit",
    props: ['movie', 'token'],
    data(){
        return{
            localMovie: {...this.movie}
        }
    },
    watch: {
        movie:function(newVal, oldVal){
            if(newVal !== oldVal){
                this.localMovie = {...this.movie}
            }
        }

    },
    methods: {
        saveMovie(){
            if(this.movie.id){
                fetch(`https://filmymt.herokuapp.com/api/movies/${this.movie.id}/`, {
                        method: 'PUT',
                        headers: {
                            'Content-Type': 'application/json',
                            'Authorization': `Token ${this.token}`
                        },
                    body: JSON.stringify({title: this.localMovie.title, description: this.localMovie.description})
                })
                .then( resp => resp.json())
                .then( () => {
                    this.$emit('update');              
                        
                })
                .catch( error => console.log(error)) 
            } else{                
                fetch(`https://filmymt.herokuapp.com/api/movies/`, {
                        method: 'POST',
                        headers: {
                            'Content-Type': 'application/json',
                            'Authorization': `Token ${this.token}`
                        },
                    body: JSON.stringify({title: this.localMovie.title, description: this.localMovie.description})
                })
                .then( resp => resp.json())
                .then( () => {
                    this.$emit('update');              
                        
                })
                .catch( error => console.log(error)) 

            }
        }
    }
}
</script>

<style  scoped>

</style>