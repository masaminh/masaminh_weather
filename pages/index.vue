<template>
  <div>
    <h2>{{ cityname }}の天気</h2>
    <div class="small">
      <b-table :items="items" :fields="fields">
        <template slot="thead-top">
          <b-tr>
            <b-td>&nbsp;</b-td>
            <b-th colspan="3" class="text-center">tenki.jp</b-th>
            <b-th colspan="3" class="text-center">weathernews</b-th>
          </b-tr>
        </template>
        <template slot="[time]" slot-scope="data">
          {{ $moment(data.value).format('MM/DD HH時') }}
        </template>
        <template slot="[tenkijp_icon]" slot-scope="data">
          <img :src="data.item.tenkijp.weather_icon" class="weather_icon" />
        </template>
        <template slot="[tenkijp_temperature]" slot-scope="data">
          {{ Math.round(data.item.tenkijp.temperature) }}
        </template>
        <template slot="[tenkijp_precipitation]" slot-scope="data">
          {{ data.item.tenkijp.precipitation }}
        </template>
        <template slot="[weathernews_icon]" slot-scope="data">
          <img :src="data.item.weathernews.weather_icon" class="weather_icon" />
        </template>
        <template slot="[weathernews_temperature]" slot-scope="data">
          {{ data.item.weathernews.temperature }}
        </template>
        <template slot="[weathernews_precipitation]" slot-scope="data">
          {{ Math.round(data.item.weathernews.precipitation) }}
        </template>
      </b-table>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      cityname: '',
      fields: [],
      items: []
    }
  },
  async asyncData({ app }) {
    const res = await app.$axios.$get(
      'https://uou83qyqb8.execute-api.ap-northeast-1.amazonaws.com/Prod/hourly/01101'
    )
    return {
      cityname: res.cityname,
      fields: [
        { key: 'time', label: '時' },
        { key: 'tenkijp_icon', label: '' },
        { key: 'tenkijp_temperature', label: '℃' },
        { key: 'tenkijp_precipitation', label: 'mm' },
        { key: 'weathernews_icon', label: '' },
        { key: 'weathernews_temperature', label: '℃' },
        { key: 'weathernews_precipitation', label: 'mm' }
      ],
      items: res.weathers
    }
  }
}
</script>
<style scoped>
img.weather_icon {
  width: 25px;
}
</style>
