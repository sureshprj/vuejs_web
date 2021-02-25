<template>
  <div class="market-data-table">
    <v-row class="header border">
      <v-col cols="10" sm="10" md="6" lg="6" xl="6" class="col-hight">
        <v-tabs
          class="tab-cls"
          background-color="transparent"
          active-class="active-tab"
        >
          <v-tabs-slider color="#00D46A" height="1px"></v-tabs-slider>
          <v-tab class="normal-tab">All Products</v-tab>
          <v-tab class="normal-tab">Futures</v-tab>
          <v-tab class="normal-tab">Options</v-tab>
          <v-tab class="normal-tab">Spot</v-tab>
        </v-tabs>
      </v-col>
      <v-col
        cols="2"
        sm="2"
        class="search-box"
        v-if="$vuetify.breakpoint.smAndDown"
      >
        <img src="../assets/search_icon.svg" />
      </v-col>
      <v-col
        cols="2"
        sm="2"
        md="6"
        lg="6"
        xl="6"
        class="search-box"
        v-if="!$vuetify.breakpoint.smAndDown"
      >
        <div class="grid-input">
          <v-text-field
            class="input"
            background-color="rgba(0, 0, 0,0.3)"
            v-model="search"
            label="Search"
            single-line
            hide-details
          >
            <template slot="append">
              <svg
                width="14"
                height="14"
                viewBox="0 0 14 14"
                fill="none"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  d="M13.5352 12.7477L10.3141 9.52656C11.1125 8.55312 11.5938 7.30625 11.5938 5.94727C11.5938 2.83008 9.06445 0.300781 5.94727 0.300781C2.82734 0.300781 0.300781 2.83008 0.300781 5.94727C0.300781 9.06445 2.82734 11.5938 5.94727 11.5938C7.30625 11.5938 8.55039 11.1152 9.52383 10.3168L12.7449 13.5352C12.9637 13.7539 13.3164 13.7539 13.5352 13.5352C13.7539 13.3191 13.7539 12.9637 13.5352 12.7477ZM5.94727 10.4699C3.45078 10.4699 1.42188 8.44102 1.42188 5.94727C1.42188 3.45352 3.45078 1.42188 5.94727 1.42188C8.44102 1.42188 10.4727 3.45352 10.4727 5.94727C10.4727 8.44102 8.44102 10.4699 5.94727 10.4699Z"
                  fill="white"
                />
              </svg>
            </template>
          </v-text-field>
        </div>
      </v-col>
    </v-row>
    <v-row v-if="isMarket">
      <v-col cols="12">
        <v-row class="market-menu">
          <v-col cols="12" class="items-list">
            <div class="menu-button">
              <v-tabs
                background-color="transparent"
                active-class="active-menu"
                :hide-slider="true"
                height="30px"
              >
                <v-tabs-slider height="0px"></v-tabs-slider>
                <v-tab class="menu-cls">All Markets</v-tab>
                <v-tab class="menu-cls">BTC Markets</v-tab>
                <v-tab class="menu-cls">ETH Markets</v-tab>
                <v-tab class="menu-cls">EMDX Markets</v-tab>
              </v-tabs>
            </div>
            <div class="flx-display checkbox-list">
              <v-checkbox
                :dense="true"
                v-model="perpetual"
                label="Perpetual"
                color="#1262FF"
              ></v-checkbox>
              <v-checkbox
                :dense="true"
                v-model="oct2020"
                label="October 2020"
                 color="#1262FF"
              ></v-checkbox>
              <v-checkbox
                :dense="true"
                v-model="nov2020"
                label="November 2020"
                 color="#1262FF"
              ></v-checkbox>
            </div>
            <div class="sort-column">
              <v-select
                :items="elements"
                v-model="sort"
                class="market-dropdown"
                item-text="name"
                item-value="name"
              >
                <template slot="selection" slot-scope="data">
                  <div class="dropdown-market">
                    <div :class="data.item.class" class="margin-15"></div>
                    <span>{{ data.item.name }}</span>
                  </div>
                </template>
                <template v-slot:item="{ item }">
                  <div class="dropdown-market-items">
                    <div :class="item.class" class="margin-right-15"></div>
                    <span>{{ item.name }}</span>
                  </div>
                </template>
              </v-select>
            </div>
          </v-col>
        </v-row>
      </v-col>
    </v-row>

    <v-data-table
      :headers="getHeader"
      :items="desserts"
      :search="search"
      class="grid-sys"
      show-expand
      itemClass="row-hieght"
      item-key="id"
    >
      <template v-slot:header.star>
        <img src="../assets/star.svg" />
      </template>
      <template v-slot:header.name="{ header }">
        {{ header.text.toUpperCase() }}
      </template>
      <template v-slot:item.btn>
        <div class="trade-btn">
          <v-btn></v-btn>
        </div>
      </template>
      <template v-slot:item.star>
        <div class="pad-left-10px">
          <img src="../assets/star.svg" />
        </div>
      </template>
      <template v-slot:item.name="{ item }">
        <span class="first-col bold">{{ item.name }}</span>
      </template>
      <template v-slot:item.change="{ item }">
        <span class="h-24-change">{{ item.change }}</span>
      </template>
      <template v-slot:item.line="{ item }">
        <div class="line-chart-grid">
          <GridLineChart :data="item"></GridLineChart>
        </div>
      </template>
      <template v-slot:item.data-table-expand="{ expand, isExpanded }">
        <img
          v-if="!isExpanded"
          src="../assets/expand.svg"
          class="expand-icon pad-right-10px"
          @click="expand(!isExpanded)"
        />
        <img
          v-if="isExpanded"
          src="../assets/collapse.svg"
          class="expand-icon-bright pad-right-10px"
          @click="expand(!isExpanded)"
        />
      </template>
      <template v-slot:expanded-item="{ headers, item }">
        <td :colspan="headers.length">More info about {{ item.name }}</td>
      </template>
    </v-data-table>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import GridLineChart from "@/components/GridLineChart.vue";
const star = `<svg width="20" height="19" viewBox="0 0 20 19" fill="none" xmlns="http://www.w3.org/2000/svg">
<path opacity="0.2" d="M10 0L12.2451 6.90983H19.5106L13.6327 11.1803L15.8779 18.0902L10 13.8197L4.12215 18.0902L6.36729 11.1803L0.489435 6.90983H7.75486L10 0Z" fill="white"/>
</svg>
`;

export const getImageURL = (image: any) => {
  const blob = image ? new Blob([image], { type: "image/svg+xml" }) : "";
  const url = blob ? URL.createObjectURL(blob) : "";
  return url;
};
const sortList: any = [
  {
    class: `sort-icon`,
    name: "Top Performers",
  },
];

export default Vue.extend({
  name: "DataTableMarket",
  components: {
    GridLineChart,
  },
  props: ["isMarket"],
  data: () => ({
    search: "",
    sort: "Top Performers",
    perpetual: true,
    oct2020: true,
    nov2020: true,
    elements: sortList,
    headers: [
      {
        text: "",
        value: "star",
        align: "center",
        class: "h-txt h-padding",
        sortable: false
      },
      {
        text: "Market",
        value: "name",
        class: "h-txt"
      },
      {
        text: "Last price",
        align: "left",
        value: "lastPrice",
        class: "h-txt",
        sortable: false
      },
      {
        text: "74h High",
        align: "left",
        value: "high",
        class: "h-txt",
        sortable: false
      },
      {
        text: "24h Low",
        align: "left",
        value: "low",
        class: "h-txt",
        sortable: false
      },
      {
        text: "24h Change",
        align: "left",
        value: "change",
        class: "h-txt",
        sortable: false
      },
      {
        text: "Max Leverage",
        align: "left",
        value: "volume",
        class: "h-txt",
        sortable: false
      },
      {
        text: "24h Volume",
        align: "left",
        value: "volume",
        class: "h-txt",
        sortable: false
      },
      {
        text: "",
        value: "line",
        align: "left",
        sortable: false
      },
      {
        text: "",
        value: "btn",
        class: "h-txt",
        sortable: false
      },
      {
        text: "",
        value: "data-table-expand",
        align: "center",
        sortable: false
      },
    ],
    desserts: [
      {
        name: "BTCUSDT PERP",
        lastPrice: 9875.43,
        high: 0.02462176,
        low: 43.45998912,
        change: "+8.32%",
        volume: "1234.01K",
        line: "1%",
        id: 1,
      },
      {
        name: "ETHUSDT PERP",
        lastPrice: 9875.43,
        high: 0.02462176,
        low: 43.45998912,
        change: "+8.32%",
        volume: "1234.01K",
        line: "1%",
        id: 2,
      },
      {
        name: "BTCUSDT PERP",
        lastPrice: 9875.43,
        high: 0.02462176,
        low: 43.459989,
        change: "+8.32%",
        volume: "1234.01K",
        line: "1%",
        id: 3,
      },
      {
        name: "BTCUSDT PERP",
        lastPrice: 9875.43,
        high: 0.024621,
        low: 43.459989,
        change: "+8.32%",
        volume: "1234.01K",
        line: "1%",
        id: 4,
      },
      {
        name: "BTCUSDT PERP",
        lastPrice: 9875.43,
        high: 0.02462176,
        low: 43.45998912,
        change: "+8.32%",
        volume: "1234.01K",
        line: "1%",
        id: 5,
      },
      {
        name: "BTCUSDT PERP",
        lastPrice: 9875.43,
        high: 0.02462176,
        low: 43.45998912,
        change: "+8.32%",
        volume: "1234.01K",
        line: "1%",
        id: 6,
      },
      {
        name: "BTCUSDT PERP",
        lastPrice: 9875.43,
        high: 0.02462176,
        low: 43.45998912,
        change: "+8.32%",
        volume: "1234.01K",
        line: "1%",
        id: 7,
      },
      {
        name: "BTCUSDT PERP",
        lastPrice: 9875.43,
        high: 0.02462176,
        low: 43.45998912,
        change: "+8.32%",
        volume: "1234.01K",
        line: "1%",
        id: 8,
      },
      {
        name: "BTCUSDT PERP",
        lastPrice: 9875.43,
        high: 0.02462176,
        low: 43.45998912,
        change: "+8.32%",
        volume: "1234.01K",
        line: "1%",
        id: 9,
      },
      {
        name: "BTCUSDT PERP",
        lastPrice: 9875.43,
        high: 0.02462176,
        low: 43.45998912,
        change: "+8.32%",
        volume: "1234.01K",
        line: "1%",
        id: 10,
      },
      {
        name: "BTCUSDT PERP",
        lastPrice: 9875.43,
        high: 0.02462176,
        low: 43.45998912,
        change: "+8.32%",
        volume: "1234.01K",
        line: "1%",
        id: 11,
      },
      {
        name: "BTCUSDT PERP",
        lastPrice: 9875.43,
        high: 0.02462176,
        low: 43.45998912,
        change: "+8.32%",
        volume: "1234.01K",
        line: "1%",
        id: 12,
      },
    ],
  }),
  computed: {
    getHeader: function () {
      if (this.isMarket) {
        let head: any = this.headers;
        if ((this.$vuetify as any).breakpoint.mdOnly) {
          head.map((col: any) => {
            if (col.value == "change") {
              col["width"] = "80px";
            }
            if (col.value == "volume") {
              col["width"] = "85px";
            }
            if (col.value == "data-table-expand") {
              col["width"] = "25px";
            }
            if (col.value == "btn") {
              col["width"] = "100px";
            }
            if (col.value == "star") {
              col["width"] = "40px";
            }
          });
        }
        if ((this.$vuetify as any).breakpoint.smAndDown) {
          head = [
            {
              text: "",
              value: "star",
              align: "center",
              class: "h-txt pad-left-10px",
              sortable: false,
              width: "20px",
            },
            {
              text: "Market / \n24h Volume",
              value: "name",
              class: "h-txt",
              width: "80px",
            },
            {
              text: "Last price /\n24h Change",
              align: "left",
              value: "lastPrice",
              class: "h-txt",
              sortable: false,
              width: "66px",
            },
            {
              text: "24h High",
              align: "left",
              value: "high",
              class: "h-txt",
              sortable: false,
              width: "58px",
            },
            {
              text: "24h Low",
              align: "left",
              value: "low",
              class: "h-txt",
              sortable: false,
              width: "55px",
            },
            {
              text: "Max Leverage",
              align: "left",
              value: "volume",
              class: "h-txt",
              sortable: false,
              width: "60px",
            },
            {
              text: "",
              value: "line",
              align: "left",
              sortable: false,
              width: "60px",
            },
            {
              text: "",
              value: "btn",
              class: "h-txt",
              sortable: false,
              width: "60px",
            },
            {
              text: "",
              value: "data-table-expand",
              align: "center",
              sortable: false,
              width: "12px"
            }
          ];
        }

        return head;
      }
      return this.headers;
    },
  },
  methods: {},
});
</script>
<style lang="scss">
$text-field-padding: 5px 0px;
.sort-icon {
  background-image: url("../assets/sort_asc_icon.svg");
  width: 14px;
  height: 14px;
}
.dropdown-market-items {
  display: flex;
  justify-content: right;
  align-items: center;
  width: 100%;
  font-size: 12px;
  line-height: 16px;
  .margin-right-15 {
    margin-right: 15px;
  }
}

.market-data-table {
  background: linear-gradient(180deg, #091a2c 0%, rgba(17, 48, 79, 0.5) 100%);
  box-shadow: 0px 0px 81.5691px rgba(0, 0, 0, 0.2);
  border-radius: 24.4707px;
  .v-data-table-header {
    th {
      border-bottom: none !important;
    }
  }
  .sort-column {
    display: flex;
    align-items: center;
    justify-content: flex-end;
    margin-left: auto;
  }
  .expand-icon {
    opacity: 0.3;
    cursor: pointer;
  }
  .expand-icon-bright {
    opacity: 1;
    cursor: pointer;
  }
  .market-dropdown {
    background-color: rgba(255, 255, 255, 0.05);
    height: 32px;
    width: 200px;
    max-width: 200px;
    padding: 0px;
    margin: 0px;
    .margin-15 {
      margin: 0px 15px;
    }

    .dropdown-market {
      display: flex;
      align-items: center;
      width: 100%;
      font-size: 12px;
      line-height: 16px;
    }
    .mdi-menu-down::before {
      content: url("../assets/lang-arrow.svg");
      top: -17px;
      position: absolute;
      right: 0px;
      opacity: 0.8;
    }
    .v-input__slot:before {
      border-color: rgba(255, 255, 255, 0.1) !important;
    }
    .v-input__control {
      height: 32px;
    }
    .v-input__slot {
      margin: 0px;
    }
  }
  .col-hight {
    padding: 0px;
    height: 59px;
    .tab-cls {
      .v-tabs-slider-wrapper {
        max-height: 1px;
      }
      .v-item-group {
        height: 60px;
      }
      .active-tab {
        font-weight: bold !important;
        text-transform: capitalize !important;
        color: var(--v-fontColor-base) !important;
      }
      .normal-tab {
        font-style: normal;
        font-weight: normal;
        font-size: 16px;
        line-height: 21px;
        color: rgba(255, 255, 255, 0.5);
        text-transform: none;
        padding: 0px 25px;
        letter-spacing: 0;
      }
    }
  }
  .border {
    border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  }
  .header {
    border-radius: 24.4707px 24.4707px 0px 0px;
    background: rgba(0, 0, 0, 0.1);
    margin: 0px;
  }
  .v-data-table-header {
    background: rgba(255, 255, 255, 0.05);
    .active {
      color: var(--v-fontColor-base) !important;
      font-weight: bold;
    }
  }
  .more-btn {
    background-color: transparent !important;
    box-shadow: 0 0 30px rgba(0, 0, 0, 0.3) !important;
    border-radius: 6px;
    border: 1px solid;
    border-color: #00d46a;
    width: 80px !important;
    height: 32px;
    font-family: "PT Sans";
    font-size: 16px !important;
    line-height: 21px;
    .v-btn__content {
      gap: 5px;
    }
    h1 {
      font-size: 12px !important;
      line-height: 21px;
      background: -webkit-linear-gradient(
        90.44deg,
        #00d46a 7.72%,
        #1262ff 126.2%
      );
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
    }
  }
  .grid-sys {
    font-family: "PT Sans";
    font-style: normal;
    font-weight: normal;
    font-size: 16px;
    line-height: 21px !important;
    color: rgba(255, 255, 255, 0.7) !important;
    background: transparent !important;
    td {
      font-size: inherit !important;
    }
    tbody {
      color: rgba(255, 255, 255, 1) !important;
      tr:hover:not(.v-data-table__expanded__content):not(.v-data-table__empty-wrapper) {
        background: rgba(0, 0, 0, 0.1) !important;
      }
    }
    .first-col {
      font-weight: bold;
    }
    .h-24-change {
      color: #00b865;
    }
    .trade-btn {
      background-image: url("../assets/trade_btn.png");
      width: 80px;
      height: 32px;
      border-radius: 6px;
      background-color: transparent !important;
      padding: 1px;
      button {
        background: transparent !important;
        width: 100% !important;
        height: 100% !important;
        padding: 1px !important;
        border-radius: 6px;
      }
    }
  }

  .search-box {
    display: flex;
    justify-content: flex-end;
    align-items: center;

    .grid-input {
      display: flex;
      align-items: flex-end;
      justify-content: flex-end;
      justify-items: center;
      padding: 0px 8px;
      .v-label {
        font-style: italic !important;
        font-weight: normal !important;
        font-size: 14px !important;
        line-height: 18px !important;
        color: rgba(255, 255, 255, 0.5);
        left: 10px !important;
      }
      .input {
        width: 200px;
        font-style: italic !important;
        font-weight: normal !important;
        font-size: 14px !important;
        line-height: 18px !important;
        color: rgba(255, 255, 255, 0.5);
      }
      .v-input__append-inner {
        margin: 0px !important;
        display: unset;
        align-self: auto;
        opacity: 0.5;
        right: 15px;
        position: absolute;
      }
      .v-text-field {
        padding: 0px !important;
        margin: 0px !important;
      }
      .v-input__slot:before {
        border-color: rgba(255, 255, 255, 0.1) !important;
      }
      .v-input__slot {
        padding: 0px 5px;
      }
      .v-input--is-focused {
        opacity: 1;
      }
    }
  }
  .h-txt {
    font-style: normal;
    font-weight: normal;
    font-size: 14px !important;
    line-height: 18px;
    text-transform: capitalize;
    color: rgba(255, 255, 255, 0.7);
    border-bottom: none !important;
  }
  .market-menu {
    align-items: center;
    padding: 0px 20px;
    .items-list {
      display: flex;
      gap: 50px;
      flex-wrap: wrap;
    }
    .menu-button {
      background: #070f18;
      border-radius: 4px;
      height: 30px;
      .menu-cls {
        font-style: normal;
        font-weight: normal !important;
        font-size: 14px !important;
        line-height: 18px !important;
        text-align: center;
        color: rgba(255, 255, 255, 0.5) !important;
        text-transform: capitalize;
        letter-spacing: 0;
        min-width: 110px;
      }
      .active-menu {
        background: #1262ff;
        border-radius: 4px;
        font-style: normal;
        font-weight: bold !important;
        font-size: 14px !important;
        line-height: 18px !important;
        text-align: center;
        color: #000000 !important;
      }
    }
    .checkbox-list {
      flex-direction: row;
      gap: 30px;
      height: 30px;
      .v-application .primary--text {
        color: #1262FF !important;
        caret-color: transparent !important;
      }
      .v-label {
        font-size: 12px;
        line-height: 16px;
        color: rgba(255, 255, 255, 0.5);
      }
      .v-icon.v-icon--dense {
        font-size: 17px;
      }
    }
  }
}

//start md screen
@media only screen and (min-width: 960px) and (max-width: 1263px) {
  .market-data-table {
    margin: 0px -12px;
    .h-txt {
      font-size: 12px !important;
      line-height: 16px !important;
    }
    .grid-sys {
      font-size: 14px;
      line-height: 18px;
    }
    .market-dropdown {
      width: 146px;
      .margin-15 {
        margin: 0px 8px;
      }
    }
    .v-input--selection-controls__input {
      width: 14px;
      height: 14px;
    }

    .market-menu {
      padding: 0px 10px;
      .items-list {
        gap: 28px;
        padding: 12px 20px;
      }
      .checkbox-list {
        gap: 12px;
      }
      .menu-button {
        .active-menu {
          font-size: 13px !important;
          line-height: 17px !important;
        }
        .menu-cls {
          font-size: 13px !important;
          line-height: 17px !important;
          min-width: 90px;
          padding: 0px 7px;
        }
      }
    }
    .v-data-table > .v-data-table__wrapper > table > tbody > tr > td,
    .v-data-table > .v-data-table__wrapper > table > tbody > tr > th,
    .v-data-table > .v-data-table__wrapper > table > thead > tr > td,
    .v-data-table > .v-data-table__wrapper > table > thead > tr > th,
    .v-data-table > .v-data-table__wrapper > table > tfoot > tr > td,
    .v-data-table > .v-data-table__wrapper > table > tfoot > tr > th {
      padding: 0px 7px;
    }
  }
}
//end md screen
//start sm screen
@media only screen and (max-width: 959px) {
  .market-data-table {
    margin: 0px -12px;
    .h-txt {
      font-size: 11px !important;
      line-height: 16px !important;
      white-space: pre-wrap;
    }
    .search-box {
      padding-right: 30px;
    }
    .v-data-table-header {
      .text-start {
        text-transform: lowercase !important;
      }
      .text-start::first-letter {
        text-transform: capitalize !important;
      }
    }
    .grid-sys {
      font-size: 12px;
      line-height: 16px;
      .trade-btn {
        width: 61px;
        background-size: 61px 32px;
        background-position: center;
        overflow: hidden;
        button {
          min-width: 61px;
        }
      }
      .first-col {
        white-space: pre;
      }
    }
    .market-dropdown {
      width: 146px;
      .margin-15 {
        margin: 0px 8px;
      }
    }
    .v-input--selection-controls__input {
      width: 14px;
      height: 14px;
    }
    .sort-column {
      margin-left: 10px;
    }
    .pad-left-10px {
      padding-left: 10px !important;
    }
    .pad-right-10px {
      padding-right: 10px !important;
    }
    .market-menu {
      padding: 0px 10px;
      .items-list {
        justify-content: center;
        align-items: center;
        gap: 28px;
        padding: 12px 20px;
      }
      .checkbox-list {
        gap: 12px;
        .v-input--selection-controls.v-input--dense:not(.v-input--switch)
          .v-input--selection-controls__ripple {
          top: calc(50% - 19px);
        }
        .v-input--selection-controls.v-input--dense
          .v-input--selection-controls__ripple {
          width: 24px;
          height: 24px;
          left: -12px;
        }
      }
      .menu-button {
        .active-menu {
          font-size: 13px !important;
          line-height: 17px !important;
        }
        .menu-cls {
          font-size: 13px !important;
          line-height: 17px !important;
          min-width: 120px;
          padding: 0px 7px;
        }
      }
    }
    .line-chart-grid {
      max-width: 70px;
      height: 35px;
      overflow: hidden;
    }
    .v-data-table > .v-data-table__wrapper > table > tbody > tr > td,
    .v-data-table > .v-data-table__wrapper > table > tbody > tr > th,
    .v-data-table > .v-data-table__wrapper > table > thead > tr > td,
    .v-data-table > .v-data-table__wrapper > table > thead > tr > th,
    .v-data-table > .v-data-table__wrapper > table > tfoot > tr > td,
    .v-data-table > .v-data-table__wrapper > table > tfoot > tr > th {
      padding: 0px 3px;
    }
  }
}
//end sm screen
</style>