<template>
  <div>
    <select
      name="location"
      id="location-list"
      v-on:change="buildChart"
      v-model="idSelected"
    >
      <option
        v-for="location in locations"
        v-bind:key="location.id"
        :value="location.id"
      >
        {{ location.dustboy_name }}
      </option>
    </select>

    <div>
      <h3 class="title">{{ stationSelected.dustboy_name }}</h3>
      <div
        id="box"
        style="position: relative; height:40vh; width:80vw"
      >
        <canvas id="myChart"></canvas>
      </div>
    </div>
  </div>
</template>

<script>
import Chart from "chart.js";
import Axios from "axios";

export default {
  name: "LocationList",
  data() {
    return {
      myChart: null,
      dusts: {
        pm10: [],
        pm25: [],
        label: []
      },
      stationSelected: [],
      rangeTime: [],
      idSelected: "2",
      locations: [
        {
          id: "2",
          dustboy_id: "DustBoy002",
          dustboy_name: "Between Cafe, ต. แม่แรม / Mae Ram, Chiang Mai",
          dustboy_lat: "18.907238",
          dustboy_lon: "98.903698"
        },
        {
          id: "3",
          dustboy_id: "DustBoy CC004",
          dustboy_name:
            "ก๋วยเตี๋ยวลีลา หน้า มร. ชม. / Chiang Mai Rajabhat University",
          dustboy_lat: "18.804701",
          dustboy_lon: "98.986802"
        },
        {
          id: "4",
          dustboy_id: "DustBoy CC005",
          dustboy_name: "เชียงใหม่ไนท์บาร์ซาร์ / Chiang Mai Night Bazaar",
          dustboy_lat: "18.7854765",
          dustboy_lon: "98.9996602"
        },
        {
          id: "5",
          dustboy_id: "DustBoy005",
          dustboy_name: "ต. ป่าแดด / Innovative Village, T. Pa Dad, Chiang Mai",
          dustboy_lat: "18.745484",
          dustboy_lon: "98.980442"
        },
        {
          id: "6",
          dustboy_id: "DustBoy006",
          dustboy_name: "มช. ต. แม่เหียะ / Chiang Mai University (Mae Hia)",
          dustboy_lat: "18.761371",
          dustboy_lon: "98.931855"
        },
        {
          id: "7",
          dustboy_id: "DustBoy CC001",
          dustboy_name: "หอการค้าเชียงใหม่ / Chiang Mai Chamber of Commerce",
          dustboy_lat: "18.752434",
          dustboy_lon: "99.030143"
        },
        {
          id: "11",
          dustboy_id: "DustBoy011",
          dustboy_name: "Cafe My Day Off เชียงดาว / Chiang doa, Chiang Mai",
          dustboy_lat: "19.455507",
          dustboy_lon: "98.961590"
        },
        {
          id: "12",
          dustboy_id: "DustBoy CC002",
          dustboy_name: "บ้านพักคนชราธรรมปกรณ์ / T. Phra Sing, Chiang Mai",
          dustboy_lat: "18.782758",
          dustboy_lon: "98.993233"
        },
        {
          id: "13",
          dustboy_id: "DustBoy H018",
          dustboy_name: "รพ. แม่แตง / Mae Taeng Hospital, Chiang Mai",
          dustboy_lat: "19.137406",
          dustboy_lon: "98.959848"
        },
        {
          id: "14",
          dustboy_id: "DustBoy H023",
          dustboy_name: "รพ. ฝาง / Fang Hospital, Chiang Mai",
          dustboy_lat: "19.913293",
          dustboy_lon: "99.206827"
        },
        {
          id: "15",
          dustboy_id: "DustBoy H019",
          dustboy_name: "รพ. พร้าว / Phrao Hospital, Chiang Mai",
          dustboy_lat: "19.367391",
          dustboy_lon: "99.200064"
        },
        {
          id: "16",
          dustboy_id: "DustBoy H013",
          dustboy_name: "รพ. ฮอด / Hod Hospital, Chiang Mai",
          dustboy_lat: "18.17944",
          dustboy_lon: "98.61197"
        },
        {
          id: "17",
          dustboy_id: "DustBoy H009",
          dustboy_name: "รพ. สันป่าตอง / San Pa Tong Hospital, Chiang Mai",
          dustboy_lat: "18.595351",
          dustboy_lon: "98.884935"
        },
        {
          id: "18",
          dustboy_id: "DustBoy H005",
          dustboy_name: "รพ. แม่ออน / Mae on Hospital, Chiang Mai",
          dustboy_lat: "18.744605",
          dustboy_lon: "99.221795"
        },
        {
          id: "19",
          dustboy_id: "DustBoy H006",
          dustboy_name: "รพ. สารภี / Sarapee Hospital, Chiang Mai",
          dustboy_lat: "18.680982",
          dustboy_lon: "99.043118"
        },
        {
          id: "21",
          dustboy_id: "DustBoy H002",
          dustboy_name: "รพ. สันทราย / Sansai Hospital, Chiang Mai",
          dustboy_lat: "18.920225",
          dustboy_lon: "98.992316"
        },
        {
          id: "22",
          dustboy_id: "DustBoy H003",
          dustboy_name: "รพ. ดอยสะเก็ด / Doi Saket Hospital, Chiang Mai",
          dustboy_lat: "18.869024",
          dustboy_lon: "99.12833"
        },
        {
          id: "23",
          dustboy_id: "DustBoy H004",
          dustboy_name: "รพ. สันกำแพง / San Kamphaeng Hospital, Chiang Mai",
          dustboy_lat: "18.715904",
          dustboy_lon: "99.122371"
        },
        {
          id: "24",
          dustboy_id: "DustBoy H007",
          dustboy_name: "รพ. หางดง / Hang Dong Hospital, Chiang Mai",
          dustboy_lat: "18.683101",
          dustboy_lon: "98.918202"
        },
        {
          id: "25",
          dustboy_id: "DustBoy H008",
          dustboy_name: "รพ. สะเมิง / Samoeng Hospital, Chiang Mai",
          dustboy_lat: "18.84665",
          dustboy_lon: "98.732204"
        },
        {
          id: "26",
          dustboy_id: "DustBoy H010",
          dustboy_name: "รพ. แม่วาง / Mae Wang Hospital, Chiang Mai",
          dustboy_lat: "18.615982",
          dustboy_lon: "98.774379"
        },
        {
          id: "28",
          dustboy_id: "DustBoy H012",
          dustboy_name: "รพ. จอมทอง / Chom Tong Hospital, Chiang Mai",
          dustboy_lat: "18.417204",
          dustboy_lon: "98.676756"
        },
        {
          id: "29",
          dustboy_id: "DustBoy H014",
          dustboy_name: "รพ. ดอยเต่า / Doi Tao Hospital, Chiang Mai",
          dustboy_lat: "17.949968",
          dustboy_lon: "98.690502"
        },
        {
          id: "30",
          dustboy_id: "DustBoy H015",
          dustboy_name: "รพ. อมก๋อย / Omkoi Hospital, Chiang Mai",
          dustboy_lat: "17.802039",
          dustboy_lon: "98.631548"
        },
        {
          id: "31",
          dustboy_id: "DustBoy H016",
          dustboy_name: "รพ. เทพรัตนฯ / Debaratana Hospital, Chiang Mai ",
          dustboy_lat: "18.498206",
          dustboy_lon: "98.379513"
        },
        {
          id: "32",
          dustboy_id: "DustBoy H017",
          dustboy_name: "รพ. วัดจันทร์ฯ / Wat Chan Hospital, Chiang Mai",
          dustboy_lat: "19.07762",
          dustboy_lon: "98.315857"
        },
        {
          id: "33",
          dustboy_id: "DustBoy H020",
          dustboy_name:
            "สสอ. เชียงดาว / Chiang Dao Public Heath Office, Chiang Mai",
          dustboy_lat: "19.366273",
          dustboy_lon: "98.964704"
        },
        {
          id: "34",
          dustboy_id: "DustBoy H021",
          dustboy_name: "รพ. เวียงแหง / Wiang Haeng Hospital, Chiang Mai",
          dustboy_lat: "19.559988",
          dustboy_lon: "98.635244"
        },
        {
          id: "35",
          dustboy_id: "DustBoy H022",
          dustboy_name: "รพ. ไชยปราการ / Chai Prakan Hospital, Chiang Mai",
          dustboy_lat: "19.705142",
          dustboy_lon: "99.136872"
        },
        {
          id: "36",
          dustboy_id: "DustBoy H024",
          dustboy_name:
            "สนง. สาธารณสุข อ. แม่อาย / Mae Ai Public Heath Office, Chiang Mai",
          dustboy_lat: "20.038608",
          dustboy_lon: "99.302391"
        },
        {
          id: "37",
          dustboy_id: "DustBoy H025",
          dustboy_name:
            "รพ. ส่งเสริมสุขภาพ เขต 1 / Health Promoting Hospital 1, Chiang Mai",
          dustboy_lat: "18.772478",
          dustboy_lon: "98.995551"
        },
        {
          id: "38",
          dustboy_id: "DustBoy H001",
          dustboy_name: "รพ. นครพิงค์ / Nakornping Hospital, Chiang Mai",
          dustboy_lat: "18.851765",
          dustboy_lon: "98.96723"
        },
        {
          id: "88",
          dustboy_id: "DustBoy088",
          dustboy_name: "สถาบันวิทยาศาสตร์สุขภาพ มช. / RIHES CMU",
          dustboy_lat: "18.795338",
          dustboy_lon: "98.958231"
        },
        {
          id: "92",
          dustboy_id: "DustBoy092",
          dustboy_name: "รร. ยุพราช / Yupparaj Wittayalai School, Chiang Mai",
          dustboy_lat: "18.792730",
          dustboy_lon: "98.987822"
        },
        {
          id: "106",
          dustboy_id: "DustBoy106",
          dustboy_name:
            "ม. ราชมงคลล้านนา / Rajamangala University of Technology Lanna",
          dustboy_lat: "18.809686",
          dustboy_lon: "98.952496"
        },
        {
          id: "108",
          dustboy_id: "DustBoy108",
          dustboy_name: " รร. เรยีนาเชลีวิทยาลัย / Regina Coeli College",
          dustboy_lat: "18.7770963",
          dustboy_lon: "99.0039795"
        },
        {
          id: "109",
          dustboy_id: "DustBoy109 ",
          dustboy_name:
            "คณะบริหารธุรกิจ มช. / Faculty of Business Administration (CMU)",
          dustboy_lat: "18.794239",
          dustboy_lon: "98.956888"
        },
        {
          id: "2004",
          dustboy_id: "True-NB-IoT-003",
          dustboy_name: "คณะวิศวกรรมศาสตร์ มช. / Faculty of Engineering (CMU)",
          dustboy_lat: "18.795943333",
          dustboy_lon: "98.953427667"
        },
        {
          id: "2005",
          dustboy_id: "True-NB-IoT-004",
          dustboy_name: "สวนบวกหาด ชม. / Suanbuakhad CNX",
          dustboy_lat: "18.783172667",
          dustboy_lon: "98.978889500"
        },
        {
          id: "2006",
          dustboy_id: "AIS-NB-IoT-001",
          dustboy_name: "บ. เอไอเอ เชียงใหม่ / AIA, Chiang mai",
          dustboy_lat: "18.780929",
          dustboy_lon: "98.977253"
        }
      ]
    };
  },

  methods: {
    buildChart() {
      this.getDetail();

      this.createChart(this.dusts.pm10, this.dusts.label);
      this.clearDust();
    },

    getDetail() {
      Axios.get(
        "https://agile-reaches-83574.herokuapp.com/api/stations/" +
          this.idSelected +
          "/avghr"
      ).then(res => {
        this.stationSelected = res.data[0];
        this.rangeTime = res.data[0].value;
        this.setDustsLabel();
        this.createDustPm10();
        this.createDustPm25();
      });
    },
    createDustPm10: function() {
      for (var i = 0; i < 10; i++) {
        this.dusts.pm10.push(Number(this.rangeTime[i].pm10));
      }
    },
    createDustPm25: function() {
      for (var i = 0; i < 10; i++) {
        this.dusts.pm25.push(Number(this.rangeTime[i].pm25));
      }
    },
    setDustsLabel: function() {
      for (var i = 0; i < 10; i++) {
        this.dusts.label.push(this.rangeTime[i].log_datetime);
      }
    },
    clearDust() {
      this.dusts.label = [];
      this.dusts.pm10 = [];
      this.dusts.pm25 = [];
    },
    createChart(val, label) {
      this.myChart == null ? "" : this.myChart.destroy();
      var ctx = document.getElementById("myChart").getContext("2d");
      this.myChart = new Chart(ctx, {
        type: "line",
        data: {
          labels: label,
          datasets: [
            {
              label: "ค่า PM10",
              data: val,
              backgroundColor: "rgba(255, 99, 132, 0.2)",
              borderColor: "rgba(255, 99, 132, 1)",
              borderWidth: 1
            }
          ]
        },
        options: {
          scales: {
            yAxes: [
              {
                ticks: {
                  beginAtZero: true
                }
              }
            ]
          }
        }
      });
    }
  },

  mounted() {
    this.createChart();
  }
};
</script>

<style>
#box {
  /* width: 80%; */
  margin: 0 auto;
}
</style>