<template>
  <Page backgroundColor="yellow">
    <ActionBar title="Яндекс" backgroundColor="black"/>
    <TabView :selectedIndex="0">
      <TabViewItem title="Яндекс.Расписание">
        <schedule-item></schedule-item>
      </TabViewItem>
      <TabViewItem title="Яндекс.Погода">
        <yandex-weather></yandex-weather>
      </TabViewItem>
      <TabViewItem title="Кинопоиск">
        <kinopoisk-item></kinopoisk-item>
      </TabViewItem>
    </TabView>
  </Page>
</template>

<script>
import ScheduleItem from './YandexSchedule.vue';
import KinopoiskItem from './Kinopoisk.vue';
import YandexWeather from './YandexWeather.vue';
  export default {
    components: { ScheduleItem, KinopoiskItem, YandexWeather }
  }
</script>

<style scoped>
  TabView {
    tab-background-color: black;
    tab-text-color: white;
    selected-tab-text-color: yellow;
    android-selected-tab-highlight-color: yellow;
  }
</style>
