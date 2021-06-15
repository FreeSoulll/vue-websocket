<template>
  <div>
     <D3LineChart :config="chart_config" :datum="temp"></D3LineChart>
  </div>
</template>

<script>
import { D3LineChart } from 'vue-d3-charts'

export default {
  components: {
    D3LineChart,
  },
  props: ['autoGraph'],
  data() {
    return {
      countClick: 0,
      timer: '',
      currentId: '',
      temp: [],
      chart_data: [],
      chart_config: {
        values: ['temperature'],
        date: {
          key: 'localTime',
          inputFormat: '%Y-%m-%d %H:%M:%S',
          outputFormat: '%Y-%m-%d %H:%M:%S',
        },
       points: {
         visibleSize: 3,
         color: 'red'
        },

        color: {
          scheme: 'schemeSet2',
        },
      },
    }
  },
  methods: {
    dataItem() {
    const myWs = new WebSocket('ws://localhost:9000')
    let date = this.temp
    // обработчик проинформирует в консоль когда соединение установится
    myWs.onopen = function () {
      console.log('подключился');
    };
    // обработчик сообщений от сервера
    myWs.onmessage =  function (message) {
      let data =  JSON.parse(message.data)
      date.push(data)
    };
  },
  },
  mounted() {
    this.dataItem()
  },

}
</script>
