<template >
    
      <StackLayout>
        <Button class="ct" text = "Выбирете город!" @tap='chose()'/>
           <StackLayout class='qwerty' orientation="vertical">
             <AbsoluteLayout>
             <Label text = " Город: " left="10" top="10" width="100" height="100"/>
             <Label class='text1' left="120" top="10"  :text='listOfItems[this.selectedItem]'/>
             </AbsoluteLayout>
           <AbsoluteLayout>
               <Label text = "Сезон: " left="10" top="10" width="100" height="100"/>
                <Label class='text1' left="120" top="10" :text='weather.fact.season'/>
             </AbsoluteLayout>

              <AbsoluteLayout>
                <Label text = "Темп: " left="10" top="10" width="100" height="100"/>
                <Label class='text1' left="120" top="10" :text='weather.fact.temp'/>
              </AbsoluteLayout>
                <AbsoluteLayout>
                   <Label text = "Ветер: " left="10" top="10" width="100" height="100"/>
                   <Label class='text1' left="120" top="10" :text='weather.fact.wind_speed'/>
              </AbsoluteLayout>
                 
           </StackLayout>
         
      </StackLayout>
   
</template>

<script >
import { Http } from '@nativescript/core'
import * as ApplicationSettings from "@nativescript/core/application-settings";
  export default {
    data() {
      return {
        listOfItems: [
           
         
          { title: "Ханты-Мансийск",
            toString: () => {
              return 'Ханты-Мансийск';
            },
            latitude: 61.004,
            longitude: 69.001
          },
          { title: "Тюмень",
            toString: () => {
              return 'Тюмень';
            },
            latitude: 61.004,
            longitude: 69.001
          },
          { title: "Нью-Йорк",
            toString: () => {
              return 'Нью-Йорк';
            },
            latitude: 40.714606, 
            longitude: -74.002800
          },
          { title: "Сургут",
            toString: () => {
              return 'Сургут';
            },
            latitude: 61.254035, 
            longitude: 73.396230
          },
          { title: "Кипр",
            toString: () => {
              return 'Кипр';
            },
            latitude: 35.169998,
            longitude:  33.359468
          },
          { title: "Москва",
            toString: () => {
              return 'Москва';
            },
            latitude: 55.753220,
            longitude:  37.622513
          },
          { title: "Нурсултан",
            toString: () => {
              return 'Нурсултан';
            },
            latitude: 51.128038, 
            longitude:  71.430307
          },
          
        ],
        selectedItem: 1,
        weather: {
            fact: {
                temp : 0,
                wind_speed : 0,
                season: 'summer,'
            }
        },
        imagePath: '',
        cities:['Ханты-Мансийск', 'Тюмень','Нью-Йорк','Сургут','Кипр','Москва','Нурсултан']
      }
    },
    mounted(){
      if(ApplicationSettings.getString('weather')){
        this.weather.fact=JSON.parse(ApplicationSettings.getString('weather'));
       
      }
      if(ApplicationSettings.getString('chose')){
        this.selectedItem=JSON.parse(ApplicationSettings.getString('chose'));
       
            }
            else{
                  this.chose()
      }
     
    },
        methods:{
      check(){
      Http.request({
        url: 'https://api.weather.yandex.ru/v2/forecast?limit=1'+'&lat=' + 
        String(this.listOfItems[this.selectedItem].latitude) + '&lon=' +
        String(this.listOfItems[this.selectedItem].longitude),
        method: "GET",
        headers: {"X-Yandex-API-Key": "2774d3ce-4acb-4d49-8852-4495f84d391b"},
        })
        .then(
        (response) => {
        this.weather = response.content.toJSON();
        
        
      });
      },
      hz(){
        alert({
          title: "Ваш заголовок",
           message: "Ваше сообщение",
        okButtonText: "Ваш текст кнопки OK"
        }).then(() => {
         console.log("Диалоговое окно закрыто");
        });
      },
      chose(){
        action("Выберете город", "Выход", this.cities)
        .then(result => {
           this.selectedItem = this.cities.indexOf(result);
           ApplicationSettings.setString('chose', JSON.stringify(this.selectedItem));        
           this.check();
           
        });
      }
    }
  }
</script>

<style>
StackLayout {
  background-color: yellow;
}
.city{

  font-size: 30;
  text-align: center;
  width: 100%;
  background-color: #ffffff;
}
.pogoda{
    background-color: #ffffff;
}

.qwerty{
  font-size: 22;
  padding: 20px;
  padding-bottom: 120px;
  background-color: #f0eaea;
  border-radius: 10%;
  margin-top:20px ;
}
.ct{
  background-color: black;
  color:white;
  padding: 50px;
  padding: 10px; 
  border-radius: 20px; 
  font-size: 16px;
}
</style>