<template>
  <div id="app">
    <div class="navBar">
      <a href="/">
        <h1 class="title">台北市颱風災害動態資訊</h1>
      </a>
      <a href="/">
        <span class="source_from">資料來源： 台北市府資訊局</span>
      </a>
    </div>

    <div class="container">
      <div class="content">
        <div class="alert-info">
          <p>防災災情及相關諮詢電話：87863119 分機 8900~8907</p>
        </div>
        <div class="select_area">
          <select name="" id="" v-model="selected">
            <option v-for="location in locations" :value="location" v-text="location"></option>
          </select>
        </div>
        <div class="table_wrap">
          <table class="table">
            <tr>
              <th width="10%">發生時間</th>
              <th width="10%">區域</th>
              <th width="15%">詳細位置</th>
              <th width="65%">描述</th>
            </tr>
            <tr v-for="(data, index) in eventData" v-if="index <= 99">
              <td v-text="data.CaseTime"></td>
              <td v-text="data.CaseLocationDistrict"></td>
              <td v-text="data.CaseLocationDescription"></td>
              <td v-text="data.CaseDescription"></td>
            </tr>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'app',
  data() {
    return {
      locations: ['-- 依區域搜尋 --', '信義區', '士林區', '中山區', '南港區', '松山區', '文山區', '北投區', '內湖區', '大安區', '萬華區', '中正區', '大同區'],
      eventData: [],
      selected: '-- 依區域搜尋 --',
    };
  },
  created() {
    this.fetchApi(this.selected);
  },
  watch: {
    selected(val) {
      this.fetchApi(val);
    },
  },
  methods: {
    fetchApi(area) {
      axios.get('https://tcgbusfs.blob.core.windows.net/blobfs/GetDisasterSummary.json')
        .then((response) => {
          if (area === '-- 依區域搜尋 --') {
            const data = response.data.DataSet['diffgr:diffgram'].NewDataSet.CASE_SUMMARY;
            this.eventData = data;
          } else {
            const data = response.data.DataSet['diffgr:diffgram'].NewDataSet.CASE_SUMMARY;
            this.eventData = [];
            for (let i = 0; i < data.length; i += 1) {
              if (data[i].CaseLocationDistrict === area) {
                this.eventData.push(data[i]);
              }
            }
          }
        });
    },
  },
};
</script>

<style>
@charset "UTF-8";
html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed,
figure, figcaption, footer, header, hgroup,
menu, nav, output, ruby, section, summary,
time, mark, audio, video {
  margin: 0;
  padding: 0;
  border: 0;
  font-weight: normal;
  background: transparent; }

body {
  line-height: 1.5; }

article, aside, details, figcaption, figure,
footer, header, hgroup, menu, nav, section {
  display: block; }

ol, ul, dl, dt, dd {
  list-style: none; }

blockquote, q {
  quotes: none; }

blockquote:before, blockquote:after,
q:before, q:after {
  content: '';
  content: none; }

table {
  border-collapse: collapse;
  border-spacing: 0; }

button, input {
  cursor: pointer;
  overflow: visible; }

button, input, optgroup, select, textarea {
  color: inherit;
  font: inherit;
  margin: 0; }

optgroup {
  font-weight: bold; }

[type="checkbox"], [type="radio"] {
  box-sizing: border-box;
  padding: 0; }

svg:not(:root) {
  overflow: hidden; }

input, button, select, textarea {
  outline: none; }

*:focus {
  outline: none; }

/*--------------------------------------------------------------------------------------------------------------------------------------------------------*/
html, body {
  width: 100%;
  height: 100%;
  min-width: 320px; }

html {
  font-family: Montserrat,Helvetica,arial,Microsoft JhengHei,"微軟正黑體",sans-serif;
  font-size: 16px; }

body {
  font-size: 1rem;
  background-color: #f7f7f7;
  -webkit-text-size-adjust: none; }

hr {
  display: block;
  height: 1px;
  border: 0;
  border-top: 1px solid #ccc;
  margin: 1rem 0;
  padding: 0; }

.clear {
  clear: both; }

/*--------------------------------------------------------------------------------------------------------------------------------------------------------*/
/*- - clearfix - -*/
.clearfix::before, .clearfix::after {
  content: '';
  display: table;
}

.clearfix::after {
  clear: both;
}

* {
  box-sizing: border-box;
}

#app {
  width: 100%;
  min-height: 100vh;
}

a {
  text-decoration: none;
}

.navBar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 10px 40px;
  background-color: #25292c;
}

.navBar .title {
  font-size: 20px;
  color: #ffc627;
}

.navBar .source_from {
  font-size: 16px;
  color: #ffc627;
}

.container {
  width: 100%;
  margin: 20px auto;
  display: flex;
  justify-content: center;
  align-items: center;
}

.content {
  width: 90%;
}

.content .alert-info {
  background-color: #25292c;
  color: #ffc627;
  padding: 20px;
}

.content .select_area {
  position: relative;
  width: 100%;
  margin: 20px auto;
  background-color: #25292c;
}

.content .select_area:after {
  position: absolute;
  color: #ffc627;
  font-family: 'Fontawesome';
  font-size: 16px;
  content: '\f0d7';
  vertical-align: middle;
  top: 7px;
  right: 15px;
  pointer-events: none;
}

.content .select_area select {
  position: relative;
  display: block;
  height: 40px;
  width: 100%;
  background-color: transparent;
  color: #ffc627;
  cursor: pointer;
  border: none;
  padding: 0 10px;
  -webkit-appearance: none;
  appearance: none;
}

.content .table {
  width: 100%;
  text-align: left;
  font-size: 85%;
}

.content .table tr > td, tr > th {
  padding: 8px;
  vertical-align: top;
}

.content .table tr > th {
  font-weight: bold;
}

.content .table tr:nth-child(even) {
  background-color: #25292c;
  color: #ffc627;
}

.content .table tr:nth-child(odd) {
  background-color: #525b60;
  color: #fff;
}

</style>

