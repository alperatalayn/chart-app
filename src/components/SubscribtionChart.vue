<template>
  <div>
    <div class="switch-wrapper">
      <div class="slider-info column">Reactivated Subscribtions</div>
      <div class="column">
        <label class="switch">
          <input
            type="checkbox"
            :value="'reactivated'"
            v-model="selectedLabels"
          />
          <span class="slider blue round"></span>
        </label>
      </div>
      <div class="slider-info column">Renewed Subscribtions</div>
      <div class="column">
        <label class="switch">
          <input type="checkbox" :value="'renewed'" v-model="selectedLabels" />
          <span class="slider red round"></span>
        </label>
      </div>
      <i class="far fa-question-circle question-icon"></i>
    </div>
    <div class="total-counts">
      <span id="total-left">{{ renewedTotal }}</span>
      <span id="total-right">{{ reactivatedTotal }}</span>
    </div>
    <div class="time-nav">
      <label class="timeframe-header">Split By:</label>
      <label v-bind:class="{ selected: timeframe == 'day' }">
        <input
          type="radio"
          value="day"
          class="hidden"
          name="day"
          v-model="timeframe"
        />
        Day
      </label>
      <label v-bind:class="{ selected: timeframe == 'week' }">
        <input
          type="radio"
          value="week"
          class="hidden"
          name="week"
          v-model="timeframe"
        />
        Week
      </label>

      <label v-bind:class="{ selected: timeframe == 'month' }">
        <input
          type="radio"
          value="month"
          class="hidden"
          name="month"
          v-model="timeframe"
        />
        Month
      </label>
      <label v-bind:class="{ selected: timeframe == 'year' }">
        <input
          type="radio"
          value="year"
          class="hidden"
          name="year"
          v-model="timeframe"
        />
        Year
      </label>
    </div>
    <LineChart
      :labels="displayedTimeframe"
      :datasets="displayedDatasets"
      :options="$options.options"
    ></LineChart>
  </div>
</template>

<script>
import LineChart from "./LineChart";

const datasetsDay = {
  reactivated: {
    borderColor: "rgba(50, 115, 220, 0.5)",
    backgroundColor: "rgba(50, 115, 220, 0.1)",
    data: [10, 45, 31, 75, 87],
  },
  renewed: {
    borderColor: "rgba(255, 56, 96, 0.5)",
    backgroundColor: "rgba(255, 56, 96, 0.1)",
    data: [70, 210, 311, 144, 125],
  },
};
const datasetsWeek = {
  reactivated: {
    borderColor: "rgba(50, 115, 220, 0.5)",
    backgroundColor: "rgba(50, 115, 220, 0.1)",
    data: [300, 700, 450, 750, 450],
  },
  renewed: {
    borderColor: "rgba(255, 56, 96, 0.5)",
    backgroundColor: "rgba(255, 56, 96, 0.1)",
    data: [6000, 5501, 7502, 2502, 7001],
  },
};
const datasetsMonth = {
  reactivated: {
    borderColor: "rgba(50, 115, 220, 0.5)",
    backgroundColor: "rgba(50, 115, 220, 0.1)",
    data: [300, 700, 450, 750, 450],
  },
  renewed: {
    borderColor: "rgba(255, 56, 96, 0.5)",
    backgroundColor: "rgba(255, 56, 96, 0.1)",
    data: [600, 550, 750, 250, 700],
  },
};
const datasetsYear = {
  reactivated: {
    borderColor: "rgba(50, 115, 220, 0.5)",
    backgroundColor: "rgba(50, 115, 220, 0.1)",
    data: [300, 200, 450, 750, 450],
  },
  renewed: {
    borderColor: "rgba(255, 56, 96, 0.5)",
    backgroundColor: "rgba(255, 56, 96, 0.1)",
    data: [600, 550, 750, 250, 700],
  },
};
const options = {
  legend: {
    display: false,
  },
  scales: {
    yAxes: [
      {
        ticks: {
          beginAtZero: true,
        },
      },
    ],
  },
};

export default {
  name: "subscribtion-chart",
  options,
  components: {
    LineChart,
  },
  data() {
    return {
      selectedLabels: ["reactivated", "renewed"],
      timeframe: "day",
    };
  },
  computed: {
    displayedDatasets() {
      if (this.timeframe === "day")
        return this.selectedLabels.map((label) => datasetsDay[label]);
      else if (this.timeframe === "week")
        return this.selectedLabels.map((label) => datasetsWeek[label]);
      else if (this.timeframe === "month")
        return this.selectedLabels.map((label) => datasetsMonth[label]);
      else return this.selectedLabels.map((label) => datasetsYear[label]);
    },
    displayedTimeframe() {
      var result = ["Mon", "Tue", "Wed", "Thu", "Fri"];
      if (this.timeframe === "day")
        result = ["Mon", "Tue", "Wed", "Thu", "Fri"];
      else if (this.timeframe === "week")
        result = ["1/01", "2/01", "3/01", "4/01", "1/02"];
      else if (this.timeframe === "month")
        result = ["Jan", "Feb", "Mar", "Apr", "May"];
      else result = ["2011", "2012", "2013", "2014", "2015"];
      return result;
    },
    renewedTotal() {
      var sum = 0;
      if (this.displayedDatasets[0]) {
        var i = this.displayedDatasets[0].data.length;
        while (i--) sum += this.displayedDatasets[0].data[i];
      }
      return sum;
    },
    reactivatedTotal() {
      var sum = 0;
      if (this.displayedDatasets[1]) {
        var i = this.displayedDatasets[1].data.length;
        while (i--) sum += this.displayedDatasets[1].data[i];
      }
      return sum;
    },
  },
};
</script>
<style scoped lang="css">
#total-left {
  padding-left: 25px;
}
#total-right {
  padding-left: 160px;
}
.total-counts {
  font-size: 30px;
  padding-bottom: 30px;
}
.timeframe-header {
  color: rgb(12, 9, 9) !important;
}
.selected {
  color: #fc6f8b !important;
}
.hidden {
  opacity: 0;
  position: absolute;
  width: 0;
}
.question-icon {
  padding-left: 60px;
  color: rgb(142, 142, 142);
}
.time-nav > label {
  color: rgb(142, 142, 142);
  padding: 0px 10px;
}
.time-nav {
  font-weight: bold;
  font-size: 10px;
  display: flex;
  padding-left: 250px;
  flex-direction: row;
  padding-bottom: 20px;
  display: inline-block;
  border-bottom: 1px solid rgb(142, 142, 142);
}
.switch-wrapper {
  display: flex;
  flex-direction: row;
  float: left;
  padding-bottom: 20px;
}

.slider-info {
  font-weight: bold;
  display: inline-block;
  font-size: 11px;
  margin: auto;
  padding-left: 30px;
  padding-right: 10px;
}
.switch {
  position: relative;
  display: inline-block;
  width: 30px;
  height: 17px;
}

.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 13px;
  width: 13px;
  left: 2px;
  bottom: 2px;
  background-color: white;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

input:checked + .blue {
  background-color: #2196f3;
}

input:focus + .blue {
  box-shadow: 0 0 1px #2196f3;
}

input:checked + .red {
  background-color: #fc6f8b;
}

input:focus + .red {
  box-shadow: 0 0 1px #fc6f8b;
}

input:checked + .slider:before {
  -webkit-transform: translateX(13px);
  -ms-transform: translateX(13px);
  transform: translateX(13px);
}

.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}
</style>