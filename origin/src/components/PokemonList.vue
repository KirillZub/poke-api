<template>
    <div class="list">
        <article v-for="(pokemon, index) in pokemons"
        :key="'poke'+index"
        @click="setPokemonUrl(pokemon.url)">
            <img :src="imageUrl + pokemon.id + '.png'" alt="">
            <h3>{{ pokemon.name }}</h3>
        </article>
    </div>
</template>

<script>
export default {
    props: [
        'imageUrl',
        'apiUrl',
    ],
    data: () => {
        return {
            pokemons: [],
            nextUrl: '',
            curentUrl: ''
        }
    },
    methods: {
        fetchData() {
            let req = new Request(this.curentUrl);
            fetch(req)
                .then((resp) => {
                    if(resp.status === 200)
                        return resp.json();
                }) 
                .then((data) => {
                    this.nextUrl = data.next;
                    data.results.forEach(pokemon => {
                        pokemon.id = pokemon.url.split('/')
                            .filter(function(part){
                                return !!part
                            }).pop();
                        this.pokemons.push(pokemon);
                    });
                })
                .catch((error) => {
                    console.log(error)
                })
        },
        next() {
            this.curentUrl = this.nextUrl;
            this.fetchData();
        },
        setPokemonUrl(url) {
            this.$emit('setPokemonUrl', url)
        }
    },
    created() {
        this.curentUrl = this.apiUrl;
        this.fetchData();
    }
}
</script>

<style lang="sass" scoped>
h3
    text-transform: uppercase
    margin-top: 30px
    font-family: Play
.list
    width: 100%
    display: flex
    justify-content: center
    flex-wrap: wrap
    margin: 0 20px 0 20px
    
    img
        width: 150px
        background-color: #E6E6FA
        border-radius: 20px
        -webkit-box-shadow: 0px 26px 16px -4px rgba(34, 60, 80, 0.36)
        -moz-box-shadow: 0px 26px 16px -4px rgba(34, 60, 80, 0.36)
        box-shadow: 0px 26px 16px -4px rgba(34, 60, 80, 0.36)
        border: 3px solid #000
        border-top: none
    article  
        background: #024191
        height: 250px
        width: 155px
        border: 3px solid black
        border-radius: 20px 
        margin: 15px 15px 15px 15px
        color: #fff
        
        &:hover
            -webkit-box-shadow: 9px 9px 16px 0px rgba(34, 60, 80, 0.65)
            -moz-box-shadow: 9px 9px 16px 0px rgba(34, 60, 80, 0.65)
            box-shadow: 9px 9px 16px 0px rgba(34, 60, 80, 0.65)
            cursor: pointer
            transition: all 0.5s

</style>