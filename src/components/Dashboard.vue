<template>
    <div>

      <div>
          <button class="site-title sites" @click="toggleLrc">ssilrc.army.mil</button>
          <div class="stats" v-show="showLrc">

            <div class="pie-chart">
          <div>
              <div>
              <b>Hourly Uptime</b><br>
              <vue3-chart-js :key="lrcPieHour" v-bind="{ ...lrcPieHour }" />
              <b>{{ lrcHourPercent }}% Uptime</b>
              </div>
          </div>
        </div>

        <div class="pie-chart">
          <div>
              <div style="max-width: 15vw">
              <b>Daily Uptime</b><br>
              <vue3-chart-js :key="lrcPieDay" v-bind="{ ...lrcPieDay }" />
              <b>{{ lrcDayPercent }}% Uptime</b>
              </div>
          </div>
      </div>

        <div class="pie-chart">
          <div>
              <div style="max-width: 15vw">
              <b>Weekly Uptime</b><br>
              <vue3-chart-js :key="lrcPieWeek" v-bind="{ ...lrcPieWeek }" />
              <b>{{ lrcWeekPercent }}% Uptime</b>
              </div>
          </div>
        </div>

        </div>
      </div>
        
      



    <div>
          <button class="site-title sites" @click="toggleBb">clle.blackboard.com</button>
          <div class="stats" v-show="showBb">

          <div class="pie-chart">
            <div>
              <div>
              <b>Hourly Uptime</b><br>
              <vue3-chart-js :key="bbPieHour" v-bind="{ ...bbPieHour }" />
              <b>{{ bbHourPercent }}% Uptime</b>
              </div>
            </div>
          </div>

        <div class="pie-chart">
          <div>
              <div>
              <b>Daily Uptime</b><br>
              <vue3-chart-js :key="bbPieDay" v-bind="{ ...bbPieDay }" />
              <b>{{ bbDayPercent }}% Uptime</b>
              </div>
          </div>
        </div>

        <div class="pie-chart">
          <div>
              <div>
              <b>Weekly Uptime</b><br>
              <vue3-chart-js :key="bbPieWeek" v-bind="{ ...bbPieWeek }" />
              <b>{{ bbWeekPercent }}% Uptime</b>
              </div>
          </div>
      </div>

        </div>
      </div>


  </div>
</template>

<script>

const fetch = require('node-fetch');
import Vue3ChartJs from "@j-t-mcc/vue3-chartjs";

export default {
  name: 'Dashboard',
  data: function () {
    return {
        cache: {},
        showLrc: false,
        showBb: false,
        lrcHourUp: 0,
        lrcHourDown: 0,
        lrcDayUp: 0,
        lrcDayDown: 0,
        lrcWeekUp: 0,
        lrcWeekDown: 0,
        bbHourUp: 0,
        bbHourDown: 0,
        bbDayUp: 0,
        bbDayDown: 0,
        bbWeekUp: 0,
        bbWeekDown: 0,

        lrcPieHour: {
            type: "pie",
            data: {
            labels: ["Up", "Down"],
            datasets: [
                {
                    backgroundColor: ["#41B883", "#E46651"],
                    data: [0, 0],
                },
            ],
        },
        
    },
    lrcPieDay: {
            type: "pie",
            data: {
            labels: ["Up", "Down"],
            datasets: [
                {
                    backgroundColor: ["#41B883", "#E46651"],
                    data: [0, 0],
                },
            ],
        },
        
    },
    lrcPieWeek: {
            type: "pie",
            data: {
            labels: ["Up", "Down"],
            datasets: [
                {
                    backgroundColor: ["#41B883", "#E46651"],
                    data: [0, 0],
                },
            ],
        },
        
    },
    bbPieHour: {
            type: "pie",
            data: {
            labels: ["Up", "Down"],
            datasets: [
                {
                    backgroundColor: ["#41B883", "#E46651"],
                    data: [0, 0],
                },
            ],
        },
        
    },
    bbPieDay: {
            type: "pie",
            data: {
            labels: ["Up", "Down"],
            datasets: [
                {
                    backgroundColor: ["#41B883", "#E46651"],
                    data: [0, 0],
                },
            ],
        },
        
    },
    bbPieWeek: {
            type: "pie",
            data: {
            labels: ["Up", "Down"],
            datasets: [
                {
                    backgroundColor: ["#41B883", "#E46651"],
                    data: [0, 0],
                },
            ],
        },
        
    },
    }
  },
  components: {
    Vue3ChartJs,
  },
  mounted() {
      this.setCache();
      setInterval(this.update, 20000);
  },
  methods: {
      update() {
          this.setCache();
          this.setLrcPieHour();
          this.setLrcPieDay();
          this.setLrcPieWeek();
          this.setbbPieHour();
          this.setbbPieDay();
          this.setbbPieWeek();
      },
      async getCache() {
        var c = await fetch('http://127.0.0.1:3300/cache');
        var response = await c.json();
        // console.log(response);
        return response;
    },
    async setCache() {
        var r = await this.getCache();
        this.cache = await r;
        this.lrcHourUp = r.lrcHour.up;
        this.lrcHourDown = r.lrcHour.down;
        this.lrcDayUp = r.lrcDay.up;
        this.lrcDayDown = r.lrcDay.down;
        this.lrcWeekUp = r.lrcWeek.up;
        this.lrcWeekDown = r.lrcWeek.down;
        this.bbHourUp = r.bbHour.up;
        this.bbHourDown = r.bbHour.down;
        this.bbDayUp = r.bbDay.up;
        this.bbDayDown = r.bbDay.down;
        this.bbWeekUp = r.bbWeek.up;
        this.bbWeekDown = r.bbWeek.down;
    },
    getPercent(up, down) {
          if(down == 0) {
              return 100;
          }
          else if(up == 0) {
              return 0;
          }
          else {
              let n = Number( 100 - ( (down * 100) / (up + down) ).toPrecision(2) );
              return n;
          }
      },
    createPie(up, down) {
        let hourUp = up;
        let hourDown = down;

        let pieChart = {
            type: "pie",
            data: {
            labels: ["Up", "Down"],
            datasets: [
                {
                    backgroundColor: ["#41B883", "#E46651"],
                    data: [0, 0],
                },
            ],
        },
        
    }
    pieChart.data.datasets[0].data[0] = hourUp;
    pieChart.data.datasets[0].data[1] = hourDown;
    return pieChart;
    },
    toggleLrc() {
        this.showLrc = !this.showLrc;
    },
    toggleBb() {
        this.showBb = !this.showBb;
    },
    setLrcPieHour() {
        this.lrcPieHour = this.createPie(this.lrcHourUp, this.lrcHourDown);
      },
      setLrcPieDay() {
        this.lrcPieDay = this.createPie(this.lrcDayUp, this.lrcDayDown);
      },
      setLrcPieWeek() {
        this.lrcPieWeek = this.createPie(this.lrcWeekUp, this.lrcWeekDown);
      },
      setbbPieHour() {
        this.bbPieHour = this.createPie(this.bbHourUp, this.bbHourDown);
      },
      setbbPieDay() {
        this.bbPieDay = this.createPie(this.bbDayUp, this.bbDayDown);
      },
      setbbPieWeek() {
        this.bbPieWeek = this.createPie(this.bbWeekUp, this.bbWeekDown);
      },
  },
  computed: {
      lrcHourPercent() {
          return this.getPercent(this.lrcHourUp, this.lrcHourDown);
      },
      lrcDayPercent() {
          return this.getPercent(this.lrcDayUp, this.lrcDayDown);
      },
      lrcWeekPercent() {
          return this.getPercent(this.lrcWeekUp, this.lrcWeekDown);
      },
      bbHourPercent() {
          return this.getPercent(this.bbHourUp, this.bbHourDown);
      },
      bbDayPercent() {
          return this.getPercent(this.bbDayUp, this.bbDayDown);
      },
      bbWeekPercent() {
          return this.getPercent(this.bbWeekUp, this.bbWeekDown);
      },
  }
}
</script>


<style scoped>
.sites {
    display:flex;
    font-size: 3vw;
    flex-direction: column;
}
.site-title {
    font-size: 3vw;
}
.stats {
    display:flex;
    flex-direction: row;
    justify-content: flex-start;
    gap: 1vw;
    max-width: 15vw;
}
.pie-chart {
    max-width: 15vw;
}

</style>
