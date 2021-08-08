<template>
    <div class="item">
        <div class="item-view">
            <div v-if="pokemon" class="image">
                <img :src="imageUrl + pokemon.id + '.png'" alt="">
            </div>
            <div v-if="pokemon" class="data">
                <h2>{{ pokemon.name }}</h2>
                <div class="property">
                    <div class="left">Количество здоровья: </div>
                    <div class="right">{{ pokemon.base_experience }} XP</div>
                </div>
                <hr>
                 <div class="property">
                    <div class="left">Высота: </div>
                    <div class="right">{{ pokemon.height/10 }} м</div>
                </div>
                <hr>
                 <div class="property">
                    <div class="left">Вес: </div>
                    <div class="right">{{ pokemon.weight/10 }} кг</div>
                </div>
                <hr>
                <h3>Способности</h3>
                <div class="abilities">
                    <div class="ability"
                    v-for="(value, index) in pokemon.abilities"
                    :key="'value'+index">
                        {{value.ability.name}}
                    </div>
                </div>
                <hr>
                <button @click="closeItem">Закрыть</button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: [
        'pokemonUrl',
        'imageUrl'
    ],
    data: () => {
        return {
            show: false,
            pokemon: {}
        }
    },
    methods: {
        fetchData() {
            let req = new Request(this.pokemonUrl);
            fetch(req)
                .then((resp) => {
                    if(resp.status === 200)
                        return resp.json();
                }) 
                .then((data) => {
                    this.pokemon = data;
                    this.show = true;
                })
                .catch((error) => {
                    console.log(error)
                })
        },
        closeItem() {
            this.$emit('closeItem');
        }
    },
    created() {
        this.fetchData();
    }
}
</script>

<style lang="sass" scoped>
.item
    display: flex
    justify-content: center
    position: fixed
    top: 0
    left: 0
    padding: 90px 10px 10px
    width: calc( 100% - 20px )
    height: calc( 100vh - 20px )
    background-color: #00000070
    font-family: Play
    .item-view
        display: flex
        justify-content: center
        align-items: center
        position: relative
        width: 100%
        height: 100%
        background-color: #fff
        max-width: 600px
        max-height: 450px
        border-radius: 20px
        border: 3px solid #000
        .image
            position: absolute
        h2
            margin-top: 100px
            text-transform: uppercase
            
        img
            margin-top: -300px
            width: 150px
            background-color: #808080
            border-radius: 100px
            -webkit-box-shadow: 0px 26px 16px -4px rgba(34, 60, 80, 0.36)
            -moz-box-shadow: 0px 26px 16px -4px rgba(34, 60, 80, 0.36)
            box-shadow: 0px 26px 16px -4px rgba(34, 60, 80, 0.36)
            display: flex
            justify-content: center
            border: 3px solid #000
            
        .data
            
            .property
                margin-top: 20px
                display: flex
                width: 400px
                justify-content: space-between
                    
            hr
                margin-top: 5px  
            h3
                margin-top: 20px
            .abilities
                display: flex
                justify-content: flex-start
                flex-wrap: wrap
                width: 400px
                margin-top: 5px
                .ability
                    padding: 5px 10px
            button
                margin-top: 20px
                font-family: Play
                padding: 5px 20px 5px 20px
                font-size: 14px
                background-color: #A52A2A
                border: 3px solid #000
                color: #fff
                cursor: pointer
                border-radius: 7px
                &:hover
                    background-color: #FF0000
                    transition: all 0.3s

</style>