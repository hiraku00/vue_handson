<script setup>
import { ref } from "vue";
import { computed } from "vue";
import Card from "./components/Card.vue";

const items = ref([
  {
    id: 1,
    name: "アボカドディップバケット",
    description:
      "刻んだ野菜をアボカドと混ぜてディップに。こんがり焼いたバゲットとお召し上がりください。",
    price: 480,
    image: "/images/item1.jpg",
    soldOut: false,
    selected: false,
  },
  {
    id: 2,
    name: "あの日夢見たホットケーキ",
    description:
      "子供のころに食べたかった、あのホットケーキを再現しました。素朴でどこか懐かしい味をどうぞ。",
    price: 1180,
    image: "/images/item2.jpg",
    soldOut: false,
    selected: false,
  },
  {
    id: 3,
    name: "HOP WTR",
    description:
      "ロサンゼルス生まれのスパークリングウォーター。ノンカロリー、ノンアルコールの新感覚飲料です。",
    price: 320,
    image: "/images/item3.jpg",
    soldOut: true,
    selected: false,
  },
  {
    id: 4,
    name: "チーズフレンチフライ",
    description:
      "イタリア産チーズをたっぷりかけたアツアツのフレンチフライ。みんな大好きな一品です。",
    price: 670,
    image: "/images/item4.jpg",
    soldOut: false,
    selected: false,
  },
]);

/**
 * 在庫のある商品数を返す
 */
function stockQuantity() {
  return items.value.filter((item) => item.soldOut === false).length;
}

const stockQuantityComputed = computed(function () {
  return items.value.filter((item) => item.soldOut === false).length;
});

/**
 * 商品の在庫状況を変更する
 * @param {object} 商品情報
 */
function stockItem(item) {
  item.soldOut = false;
}

/**
 * 現在時刻を取得する
 */
function getDate() {
  // return Date.now()
  return Date.now().toString();
}

const getDateComputed = computed(function () {
  // return Date.now()
  return Date.now().toString();
});

function alertDialog() {
  window.alert("keyup");
}

function changeSoldOut(id) {
  const pickElm = items.value.find((item) => item.id == id);
  pickElm.soldOut = true;
}
</script>

<template>
  <div>
    <header class="header">
      <img src="/images/logo.svg" alt="" />
      <h1>Vue.js ハンズオン</h1>
    </header>
    <!-- <div>商品数：{{ stockQuantity() }}</div> -->
    <div>商品数：{{ stockQuantityComputed }}</div>
    <!-- <div>現在時刻：{{ getDate() }}</div> -->
    <div>現在時刻：{{ getDateComputed }}</div>
    <!-- <input v-on:keyup.enter="alertDialog" /> -->
    <input @keyup.enter="alertDialog" />
    <main class="main">
      <template v-for="item in items" :key="item.id">
        <div
          v-if="!item.soldOut"
          class="item"
          :class="{ 'selected-item': item.selected }"
          @click="item.selected = !item.selected"
          @keyup.enter="item.selected = !item.selected"
          tabindex="0"
        >
          <Card
            :id="item.id"
            :image="item.image"
            :name="item.name"
            :description="item.description"
            :price="item.price"
            @sold-out="changeSoldOut"
          />
        </div>
        <div v-else>
          <p>売り切れです</p>
          <button type="button" @click="stockItem(item)">入荷</button>
        </div>
      </template>
    </main>
  </div>
</template>

<style>
body {
  background-color: #ffffff;
  font-family: sans-serif;
  margin: 0;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#app {
  width: 90%;
  margin: 0 5%;
  color: #242424;
}

.header {
  display: flex;
  align-content: center;
  align-items: center;
  margin-top: 40px;
  margin-bottom: 40px;
}

.header > img {
  width: 100px;
  height: 100px;
  margin-right: 20px;
}

.header > h1 {
  font-size: 80px;
  font-weight: bold;
  line-height: 80px;
  margin-top: 0;
  margin-bottom: 0;
}

.main {
  display: grid;
  grid-template-columns: 3fr 3fr 3fr 3fr;
  column-gap: 24px;
  row-gap: 24px;
}

.item {
  padding: 10px;
  cursor: pointer;
}

.item:hover {
  transition: 0.2s transform ease-out;
  transform: scale(1.05);
}

.selected-item {
  background-color: #e3f2fd;
}
</style>
