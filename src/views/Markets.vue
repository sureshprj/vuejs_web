<template>
  <div class="markets">
    <v-row class="section-market max-width-screen">
      <v-col cols="12" class="our-market">ALL OUR MARKETS</v-col>
      <v-col cols="12">
        <DataTable isMarket="true"></DataTable>
      </v-col>
    </v-row>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import DataTable from "../components/DataTable.vue";
export default Vue.extend({
  name: "Markets",
  components: {
    DataTable
  },
  data: () => {
    return {
      options: {},
    };
  },
  computed: {
    marketData: function () {
      const list = [
        {
          id: 0,
          title: "DolArBTC Index Price",
          color: "#00D46A",
          data: [
            {
              name: "series1",
              data: [
                [new Date("10-10-20"), 40],
                [new Date(), 120],
              ],
            },
          ],
        },
        {
          id: 1,
          title: "Gas Price Index",
          color: "#1262FF",
          data: [
            {
              name: "series1",
              data: [
                [new Date("10-10-20"), 40],
                [new Date(), 120],
              ],
            },
          ],
        },
        {
          id: 2,
          title: "DolArBTC Index Price",
          color: "#00D46A",
          data: [
            {
              name: "series1",
              data: [
                [new Date("10-10-20"), 40],
                [new Date(), 120],
              ],
            },
          ],
        },
        {
          id: 3,
          title: "Gas Price Index",
          color: "#00D46A",
          data: [
            {
              name: "series1",
              data: [
                [new Date("10-10-20"), 40],
                [new Date(), 120],
              ],
            },
          ],
        },
      ];
      return list;
    }
  }
});
</script>
<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Red+Hat+Text&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Roboto&display=swap");
@import url("https://fonts.googleapis.com/css2?family=PT+Sans&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Red+Hat+Display&display=swap");
@import url("https://fonts.googleapis.com/css2?family=PT+Mono&display=swap");

.markets-header-bg {
  background-image: url("../assets/market_header_bg.svg");
  background-size: cover;
}
.markets {
  background-image: url("../assets/market-bg-top.svg"),
    url("../assets/market-bg-top-poly.svg"),
    url("../assets/market-bg-bottom-poly.svg"),
    url("../assets/market-bg-bottom.svg"), url("../assets/market_grad_bg.svg");
  background-position: left 40px top 40px, right top, left bottom,
    right bottom 42px, left top 260px;
  padding-bottom: 90px;
  background-size: auto, auto, auto, auto, cover;
  .data-table {
    margin: 0px 0px;
  }

  .max-width-screen {
    margin: 0px 5%;
  }
  .desc {
    text-align: center;
    font-style: normal;
    font-weight: normal;
    font-size: 18px;
    line-height: 23px;
    display: flex;
    align-items: center;
    opacity: 0.7;
    padding: 55px 0px 0px 0px;
    margin: 0px 5%;
  }
  .line-chart-section {
    padding: 34px 0px 0px 0px;
    flex-wrap: wrap;
    gap: 20px;
  }
  .head-txt {
    padding-top: 67px;
  }
  .our-market {
    font-style: normal;
    font-weight: bold;
    font-size: 24px;
    line-height: 31px;
    color: #1262ff;
    padding: 42px 12px 20px 12px;
  }
}
//start lg screen
@media only screen and (min-width: 1264px) and (max-width: 1904px) {
  .markets {
    .max-width-screen {
      margin: 0px 5%;
    }
    .desc {
      margin: 0px 5%;
    }
  }
  @media only screen and (min-width: 1264px) and (max-width: 1360px) {
    .markets {
      .max-width-screen {
        margin: 0px 2%;
      }
      .desc {
        margin: 0px 5%;
      }
    }
  }
}
//end lg screen

//start md screen
@media only screen and (min-width: 960px) and (max-width: 1263px) {
  .markets {
    background-position: left 40px top 40px, right -42px top 0px,
      left -36px bottom, right bottom 42px, left top 260px;
    .max-width-screen {
      margin: 0px 3.2%;
    }
    .desc {
      font-size: 18px;
      line-height: 23px;
      margin: 0px 5%;
    }
    .our-market {
      padding: 77px 12px 20px 12px;
    }
    .line-chart-section {
      padding: 46px 0px 0px 0px;
      flex-wrap: wrap;
      gap: 30px;
    }
  }
  @media only screen and (min-width: 960px) and (max-width: 1000px) {
    .markets {
      .max-width-screen {
        margin: 0px 2%;
      }
      .desc {
        margin: 0px 3%;
      }
    }
  }
}
//end md screen


//start Sm screen
@media only screen and (max-width: 959px) {
  .markets {
    background-position: left 40px top 40px, right -42px top 0px,
      left 0px bottom, right bottom 42px, left top 260px;
    .app-head-txt {
      font-size: 36px;
      line-height: 48px;
    }
    .max-width-screen {
      margin: 0px 3.2%;
    }
    .desc {
      font-size: 16px;
      line-height: 21px;
      margin: 0px 5%;
    }
    .our-market {
      padding: 50px 12px 20px 12px;
    }
    .line-chart-section {
      padding: 46px 0px 0px 0px;
      flex-wrap: wrap;
      gap: 30px;
    }
  }
}
</style>
