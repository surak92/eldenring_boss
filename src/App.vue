<template>
  <header id="header" role="banner">
    <div clss="search">
      <h1 class="title">-엘- 속성 저항</h1>
      <input
        class="search-input"
        type="text"
        placeholder="보스 이름을 입력하세요."
        v-model="inputValue"
        @input="searchBoss($event)"
      />
    </div>
    <div class="immunity">
      <h3 class="imTitle">면역 속성 검색</h3>
      <input type="checkbox" value="all" v-model="allSelected" @change="bossImmune()" />
      <label for="all">전체&nbsp;&nbsp;</label>
      <br />
      <input
        type="checkbox"
        id="poison"
        value="poison"
        v-model="checkedNames"
        @change="bossImmune()"
      />
      <label for="poison">독&nbsp;</label>
      <input
        type="checkbox"
        id="scarletrot"
        value="scarletrot"
        v-model="checkedNames"
        @change="bossImmune()"
      />
      <label for="scarletrot">부패&nbsp;</label>
      <input
        type="checkbox"
        id="hemorrhage"
        value="hemorrhage"
        v-model="checkedNames"
        @change="bossImmune()"
      />
      <label for="hemorrhage">출혈&nbsp;</label>
      <input
        type="checkbox"
        id="frostbite"
        value="frostbite"
        v-model="checkedNames"
        @change="bossImmune()"
      />
      <label for="frostbite">동상&nbsp;</label>
      <input
        type="checkbox"
        id="sleep"
        value="sleep"
        v-model="checkedNames"
        @change="bossImmune()"
      />
      <label for="sleep">수면&nbsp;</label>
    </div>
  </header>
  <main id="main">
    <div>
      <table class="responsive-table">
        <thead>
          <tr>
            <th scope="col" @click="sortBack()">보스 명</th>
            <th scope="col" @click="sort('standard', 0)">
              {{ this.label[0] }}
            </th>
            <th scope="col" @click="sort('slash', 1)">{{ this.label[1] }}</th>
            <th scope="col" @click="sort('strike', 2)">{{ this.label[2] }}</th>
            <th scope="col" @click="sort('thrust', 3)">{{ this.label[3] }}</th>
            <th scope="col" @click="sort('magic', 4)">{{ this.label[4] }}</th>
            <th scope="col" @click="sort('fire', 5)">{{ this.label[5] }}</th>
            <th scope="col" @click="sort('lightning', 6)">
              {{ this.label[6] }}
            </th>
            <th scope="col" @click="sort('holy', 7)">{{ this.label[7] }}</th>
            <th scope="col">독</th>
            <th scope="col">부패</th>
            <th scope="col">출혈</th>
            <th scope="col">동상</th>
            <th scope="col">수면</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in bossList" :key="item" class="boss-item">
            <th scope="row">{{ item.name }}</th>
            <td data-title="물리 저항">{{ item.standard }}</td>
            <td data-title="참격 저항">{{ item.slash }}</td>
            <td data-title="타격 저항">{{ item.strike }}</td>
            <td data-title="관통 저항">{{ item.thrust }}</td>
            <td data-title="마력 저항">{{ item.magic }}</td>
            <td data-title="화염 저항">{{ item.fire }}</td>
            <td data-title="번개 저항">{{ item.lightning }}</td>
            <td data-title="신성 저항">{{ item.holy }}</td>
            <td data-title="독 저항">{{ item.poison }}</td>
            <td data-title="부패 저항">{{ item.scarletrot }}</td>
            <td data-title="출혈 저항">{{ item.hemorrhage }}</td>
            <td data-title="냉기 저항">{{ item.frostbite }}</td>
            <td data-title="수면 저항">{{ item.sleep }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </main>
  <footer id="footer">
    Sources:
    <a
      href="https://gall.dcinside.com/mgallery/board/view?id=fromsoftware&no=3336194"
      rel="external"
      target="_blank"
      >본편 보스</a
    >
    &amp;
    <a
      href="https://gall.dcinside.com/mgallery/board/view/?id=fromsoftware&no=4612429"
      rel="external"
      target="_blank"
      >DLC 보스</a
    >. Data is current as of january 1, 2025
  </footer>
</template>

<script>
import data from "@/assets/boss.json";

const bossListOriginal = [...data];
const OrderOriginal = [0, 0, 0, 0, 0, 0, 0, 0];
const labelOriginal = [
  "표준",
  "참격",
  "타격",
  "관통",
  "마력",
  "화염",
  "벼락",
  "신성",
];

export default {
  name: "App",
  data() {
    return {
      inputValue: "",
      OrderOriginal,
      labelOriginal,
      bossListOriginal,
      bossList: data,
      label: ["표준", "참격", "타격", "관통", "마력", "화염", "벼락", "신성"],
      Order: [0, 0, 0, 0, 0, 0, 0, 0],
      checkList: ["poison", "scarletrot", "hemorrhage", "frostbite", "sleep"],
      checkedNames: [],
    };
  },
  computed: {
    allSelected: {
      //getter
      get: function () {
        return this.checkList.length === this.checkedNames.length;
      },
      //setter
      set: function (e) {
        this.checkedNames = e ? this.checkList : [];
      },
    },
  },
  methods: {
    searchBoss(event) {
      const len = this.bossListOriginal.length;
      for (let i = 0; i < len; i++) {
        if (
          this.bossListOriginal[i].name
            .replace(/\s/g, "")
            .includes(event.target.value.replace(/\s/g, "")) === false
        ) {
          document.querySelectorAll(".boss-item")[i].style.display = "none";
        } else {
          document.querySelectorAll(".boss-item")[i].style.display = "";
        }
      }
    },
    bossImmune() {
      const imLen = this.checkedNames.length;
      const len = this.bossListOriginal.length;
      this.inputValue = "";
      switch (imLen) {
        case 1:
          for (let i = 0; i < len; i++) {
            if (
              this.bossListOriginal[i][this.checkedNames[0]].includes(
                "면역"
              ) === false
            ) {
              document.querySelectorAll(".boss-item")[i].style.display = "none";
            } else {
              document.querySelectorAll(".boss-item")[i].style.display = "";
            }
          }
          break;
        case 2:
          for (let i = 0; i < len; i++) {
            if (
              this.bossListOriginal[i][this.checkedNames[0]].includes("면역") &&
              this.bossListOriginal[i][this.checkedNames[1]].includes(
                "면역"
              ) === true
            ) {
              document.querySelectorAll(".boss-item")[i].style.display = "";
            } else {
              document.querySelectorAll(".boss-item")[i].style.display = "none";
            }
          }
          break;
        case 3:
          for (let i = 0; i < len; i++) {
            if (
              this.bossListOriginal[i][this.checkedNames[0]].includes("면역") &&
              this.bossListOriginal[i][this.checkedNames[1]].includes("면역") &&
              this.bossListOriginal[i][this.checkedNames[2]].includes(
                "면역"
              ) === true
            ) {
              document.querySelectorAll(".boss-item")[i].style.display = "";
            } else {
              document.querySelectorAll(".boss-item")[i].style.display = "none";
            }
          }
          break;
        case 4:
          for (let i = 0; i < len; i++) {
            if (
              this.bossListOriginal[i][this.checkedNames[0]].includes("면역") &&
              this.bossListOriginal[i][this.checkedNames[1]].includes("면역") &&
              this.bossListOriginal[i][this.checkedNames[2]].includes("면역") &&
              this.bossListOriginal[i][this.checkedNames[3]].includes(
                "면역"
              ) === true
            ) {
              document.querySelectorAll(".boss-item")[i].style.display = "";
            } else {
              document.querySelectorAll(".boss-item")[i].style.display = "none";
            }
          }
          break;
        case 5:
          for (let i = 0; i < len; i++) {
            if (
              this.bossListOriginal[i][this.checkedNames[0]].includes("면역") &&
              this.bossListOriginal[i][this.checkedNames[1]].includes("면역") &&
              this.bossListOriginal[i][this.checkedNames[2]].includes("면역") &&
              this.bossListOriginal[i][this.checkedNames[3]].includes("면역") &&
              this.bossListOriginal[i][this.checkedNames[4]].includes(
                "면역"
              ) === true
            ) {
              document.querySelectorAll(".boss-item")[i].style.display = "";
            } else {
              document.querySelectorAll(".boss-item")[i].style.display = "none";
            }
          }
          break;
        default:
          for (let i = 0; i < len; i++) {
            document.querySelectorAll(".boss-item")[i].style.display = "";
          }
          break;
      }
    },
    sort(key, num) {
      if (this.Order[num] == 0) {
        this.sortBack();
        this.Order[num] = -1;
        this.label[num] = this.label[num].concat("▲");
        this.bossList.sort(function (a, b) {
          return a[key] - b[key];
        });
      } else if (this.Order[num] == 1) {
        this.Order[num] *= -1;
        this.label[num] = this.label[num].replace(/▼/g, "▲");
        this.bossList.sort(function (a, b) {
          return a[key] - b[key];
        });
      } else {
        this.Order[num] *= -1;
        this.label[num] = this.label[num].replace(/▲/g, "▼");
        this.bossList.sort(function (a, b) {
          return b[key] - a[key];
        });
      }
    },
    sortBack() {
      this.bossList = [...this.bossListOriginal];
      this.Order = [...this.OrderOriginal];
      this.label = [...this.labelOriginal];
    },
  },
};
</script>

<style>
* {
  padding: 0;
  margin: 0 auto;
}
#header {
  position: fixed;
  display: flex;

  width: 100%;
  height: 130px;
  background: white;
  z-index: 999;
  top: 0;
  left: 0;
}
#main {
  margin-top: 130px;
  margin-left: 10px;
  margin-right: 10px;
}
#footer {
  padding: 10px;
  margin: 10px;
}
.responsive-table {
  width: 100%;
  margin-bottom: 1.5em;
  overflow: auto;
}
.search {
  width: 100%;
  display: inline;
}
.title {
  display: block;
  padding: 4px 8px;
  margin: 10px auto;
  outline: none;
}
.search-input {
  padding: 4px 8px;
  margin: 1%;
  width: 200px;
  font-size: 16px;
  outline: none;
}
.immunity {
  display: block;
  padding: 4px 8px;
  margin: 10px auto;
  outline: none;
}
.imTitle {
  margin: 1%;
  font-size: 26px;
  outline: none;
  margin-bottom: 10px;
}
.responsive-table thead {
  position: absolute;
  clip: rect(1px 1px 1px 1px);
  /* IE6, IE7 */
  clip: rect(1px, 1px, 1px, 1px);
  padding: 0;
  border: 0;
  height: 1px;
  width: 1px;
}
.responsive-table thead th {
  background-color: #1d96b2;
  border: 1px solid #1d96b2;
  font-weight: normal;
  text-align: center;
  color: white;
  overflow: auto;
  position: -webkit-sticky;
  position: sticky;
  top: 130px;
  cursor: pointer;
  text-align: center;
}
.responsive-table tbody,
.responsive-table tr,
.responsive-table th,
.responsive-table td {
  display: block;
  padding: 0;
  text-align: left;
  white-space: normal;
}
.responsive-table th,
.responsive-table td {
  padding: 0.5em;
  vertical-align: middle;
}
.responsive-table caption {
  margin-bottom: 1em;
  font-size: 1em;
  font-weight: bold;
  text-align: center;
}
.responsive-table tfoot {
  font-size: 0.8em;
  font-style: italic;
}
.responsive-table tbody tr {
  margin-bottom: 1em;
  border: 2px solid #1d96b2;
}
.responsive-table tbody tr:last-of-type {
  margin-bottom: 0;
}
.responsive-table tbody th[scope="row"] {
  background-color: #1d96b2;
  color: white;
}
.responsive-table tbody td[data-type="currency"] {
  text-align: right;
}
.responsive-table tbody td[data-title]:before {
  content: attr(data-title);
  float: left;
  font-size: 0.8em;
  color: rgba(94, 93, 82, 0.75);
}
.responsive-table tbody td {
  text-align: right;
  border-bottom: 1px solid #1d96b2;
}

@media (min-width: 52em) {
  .responsive-table {
    font-size: 0.9em;
  }
  .responsive-table thead {
    position: relative;
    clip: auto;
    height: auto;
    width: auto;
    overflow: auto;
  }
  .responsive-table tr {
    display: table-row;
  }
  .responsive-table th,
  .responsive-table td {
    display: table-cell;
    padding: 0.5em;
  }

  .responsive-table caption {
    font-size: 1.5em;
  }
  .responsive-table tbody {
    display: table-row-group;
  }
  .responsive-table tbody tr {
    display: table-row;
    border-width: 1px;
  }
  .responsive-table tbody tr:nth-of-type(even) {
    background-color: rgba(94, 93, 82, 0.1);
  }
  .responsive-table tbody th[scope="row"] {
    background-color: transparent;
    color: #5e5d52;
    text-align: left;
  }
  .responsive-table tbody td {
    text-align: center;
  }
  .responsive-table tbody td[data-title]:before {
    content: none;
  }
}
@media (min-width: 62em) {
  .responsive-table {
    font-size: 1em;
  }
  .responsive-table th,
  .responsive-table td {
    padding: 0.75em 0.5em;
  }
  .responsive-table tfoot {
    font-size: 0.9em;
  }
}

@media (min-width: 75em) {
  .responsive-table th,
  .responsive-table td {
    padding: 0.75em;
  }
}
</style>
