<template>
    <Page xmlns="http://schemas.nativescript.org/tns.xsd" xmlns:VideoPlayer="nativescript-videoplayer">
        <StackLayout v-if="this.trailer_ready == true">
            <Video id="nativeVideoPlayer"
            controls="true" loop="true" autoplay="false" height="280" :src="this.trailer" />
            <!-- Remote file to test with https://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4 -->
        </StackLayout>
    </Page>    
</template>
<script>
import { Http } from '@nativescript/core';
import Vue from 'nativescript-vue'
import { Video } from 'nativescript-videoplayer';
export default {
    components: { Video },
    props: {
        id: Number
    },

    data: function() {
        return {
            token: "06e18309-a747-43c7-9ad4-f9db545a52af",
            domen: "https://kinopoiskapiunofficial.tech/",
            film_id: 0,
            film_trailer: null,
            trailer: '',
            trailer_ready: false,

        }
    },

    methods: {
        getTrailer() {

            this.film_id = this.id
            Http.request({
                url: this.domen + 'api/v2.1/films/' + this.film_id + '/videos',
                method: 'GET',
                headers: {"X-API-KEY": this.token}
            }).then(
                (responce) => {
                    this.film_trailer = responce.content.toJSON();
                    let i = 0
                    this.film_trailer.trailers.forEach(el => {
                        if(i == 0){
                            this.trailer = el.url
                        }

                        i++
                    });

                    console.log(this.trailer)
                    this.trailer_ready = true

                }
            ).catch( e => {
                //action сделать чтобы говорил что нету трейлеров
            })
        }
    },

    mounted() {
        this.getTrailer()
    }
    
}
</script>
<style scoped>

</style>