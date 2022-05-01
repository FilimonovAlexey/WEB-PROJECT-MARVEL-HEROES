<template>
    <div id="app">

        <app-header :characterSearch="characterSearch" />

        <div class="container">
            <h1 class="pt-3 pb-3">Персонажи Marvel</h1>

            <app-modal :character="filterCharacters[characterIndex]"/>

            <spinner v-if="loading" />

            <div class="row">
                <div class="row">
        <div v-for="(el, idx) in filterCharacters"
             :key="el.id"
            class="card mb-3 col-sm-12 col-md-6 col-lg-4">
            <div class="row g-0">
                <div class="col-4">
                    <img :src="el.thumbnail"
                         :alt="el.name"
                         style="max-width: 100%;"
                    >
                </div>
                <div class="col-8">
                    <div class="card-body">
                        <h5 class="card-title">{{el.name}}</h5>
                        <button type="button"
                                data-bs-toggle="modal"
                                data-bs-target="#exampleModal"
                                class="btn btn-secondary btn-sm"
                                @click="characterIndex = idx"
                        >
                            Подробнее
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
            </div>
        </div>

    </div>
</template>

<script>
    import Spinner from "./components/Spinner";
    import AppModal from "./components/AppModal";
    import AppHeader from "./components/AppHeader";

    export default {
        name: 'App',
        components: {
            AppHeader,
            AppModal,
            Spinner,
        },
        data() {
            return {
                loading: false,
                characters: [],
                characterIndex: 0,
                search: '',
            }
        },
        methods: {
            fetchcharacters: function(){
                return fetch('https://netology-api-marvel.herokuapp.com/characters')
                        .then(res => res.json())
                        .then(json => this.characters = json)
            },
            characterSearch: function(value){
                this.search = value
            },
        },
        computed: {
            filterCharacters: function(){
                let {search, characters} = this
                return characters.filter(character => {
                    return character.name.toLowerCase()
                        .indexOf(search.toLowerCase()) !== -1
                })
            },
        },
        async mounted() {
            this.loading = true
            await this.fetchcharacters()
            this.loading = false
        },
    }
</script>

<style>

</style>
