<template>
  <div>
    <input
    class="search-input"
      type="text"
      placeholder="보스 이름을 입력하세요."
      @input="searchGroup($event)"
    />

    <ul class="boss-list">
      <li class="boss-list-header">
        <span>이름</span>
        <span>표준</span>
        <span>참격</span>
        <span>타격</span>
        <span>관통</span>
        <span>마력</span>
        <span>화염</span>
        <span>벼락</span>
        <span>신성</span>
        <span>독</span>
        <span>부패</span>
        <span>출혈</span>
        <span>동상</span>
        <span>수면</span>
      </li>
      <li v-for="boss in bossList" :key="boss" class="boss-item">
        <span>{{ boss.name }}</span>
        <span>{{ boss.standard }}</span>
        <span>{{ boss.slash }}</span>
        <span>{{ boss.strike }}</span>
        <span>{{ boss.thrust }}</span>
        <span>{{ boss.magic }}</span>
        <span>{{ boss.fire }}</span>
        <span>{{ boss.lightning }}</span>
        <span>{{ boss.holy }}</span>
        <span>{{ boss.poison }}</span>
        <span>{{ boss.scarletrot }}</span>
        <span>{{ boss.hemorrhage }}</span>
        <span>{{ boss.frostbite }}</span>
        <span>{{ boss.sleep }}</span>
      </li>
    </ul>
  </div>
</template>

<script>
import data from "@/assets/boss.json";

const bossList = data;

export default {
  data() {
    return {
      bossList,
    };
  },

  methods: {
    searchGroup(event) {
      // console.log(event.target.value);
    
      const len = this.bossList.length;

      for (let i = 0; i < len; i++) {
        if (((this.bossList[i].name).replace(/\s/g, '')).includes((event.target.value).replace(/\s/g, '')) === false) {
          document.querySelectorAll(".boss-item")[i].style.display = "none";
        } else {
          document.querySelectorAll(".boss-item")[i].style.display = "flex";
        }
      }
    },
  },
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
}
.search-input {
  display: block;
  padding: 4px 8px;
  margin: 10px auto;
  width: 320px;
  font-size: 16px;
  outline: none;
}

.boss-list {
  margin: 0 auto;
  width: 360px;
}

.boss-list li {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 32px;
  list-style-type: none;
}

.boss-list span {
  display: block;
  width: 33%;
  text-align: center;
}

.boss-list-header {
  font-weight: 700;
  border-bottom: 1px solid #bdbdbd;
}
</style>
