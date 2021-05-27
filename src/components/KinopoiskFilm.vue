<template>
    <Page>
        <ActionBar :title="name" backgroundColor="black"/>
        <ScrollView orientation="vertical">
            <StackLayout>
                <FlexboxLayout justifyContent="flex-start">
                    <Button text="Назад" @tap="$navigateBack" />
                </FlexboxLayout>
                <FlexboxLayout justifyContent="flex-start">
                    <Image :src="this.image"  /> 
                    <Label v-model="year" />
                    <Label v-model="film_length" />
                    <Label v-model="rating" />
                    <Label v-model="rating_age_limit" />
                </FlexboxLayout>
                <FlexboxLayout>
                    <Label class="description" editable="false" v-model="this.description" />
                </FlexboxLayout>
                <!--<Button text="Трейлер" @tap="filmTrailer()" />-->
            </StackLayout>
        </ScrollView>
    </Page>
</template>
<script>
import { Http } from '@nativescript/core';
import KinopoiskTrailer from './KinopoiskTrailer.vue';
export default {
    props: {
        id: Number
    },
    data: function(){
        return {
            token: "06e18309-a747-43c7-9ad4-f9db545a52af",
            domen: "https://kinopoiskapiunofficial.tech/",
            film_id: 0,
            data: null,
            name: '',
            image: '',
            year: '-',
            film_length: '-',
            description: '-',
            film_country: [],
            film_genre: [],
            rating: '-',
            rating_age_limit: '-',
        }
    },

    methods: {
        getInformationFilm() {
            this.film_id = this.id
            Http.request({
                url: this.domen + 'api/v2.1/films/' + this.film_id,
                method: 'GET',
                headers: {"X-API-KEY": this.token}
                
            }).then(
                (responce) => {
                    console.log(responce)
                    this.data = responce.content.toJSON();
                    this.name = this.data.data.nameRu
                    this.image = this.data.data.posterUrl
                    this.year = this.data.data.year
                    this.film_length = this.data.data.filmLength
                    this.description = this.data.data.description
                    this.data.data.countries.forEach(el => {
                        this.film_country.push(el.country)
                    });
                    this.data.data.genres.forEach(el => {
                        this.film_genre.push(el.genre)
                    });
                    this.rating = this.data.data.rating.rating
                    this.rating_age_limit = this.data.data.ratingAgeLimits
                    //this.country = this.data.data.countries
                }
            )
        },

        // Трейлер фильма
        filmTrailer(){
            this.$navigateTo(KinopoiskTrailer, {props: {
                id: this.film_id
            }})
        }
    },


    mounted() {
        this.getInformationFilm()
    }
}
</script>
<style scoped>
    Page {
        background-color: yellow;
    }
    
    Image {
        width: 200;
        height: 150;
    }

    Label {
        color:black;
        font-size: 16;   
    }

    Button {
        border-radius: 10;
        background-color: black ;
        color: white;
        font-size: 16;
    }
</style>