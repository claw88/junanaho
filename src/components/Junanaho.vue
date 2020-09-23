<template>
  <div>
    <div>
      <h3>P2の手牌（{{ p2Hand.length }}）</h3>
      <draggable v-model="p2Hand" group="myGroup" :options="options">
        <div
          class="tile"
          v-for="tile in p2Hand"
          :key="tile.id"
          :class="isFixed(false)"
        >
          {{ tile.name }}
        </div>
      </draggable>
    </div>
    <div>
      <h3>P2の打牌候補（{{ p2Pool.length }}）</h3>
      <draggable v-model="p2Pool" group="myGroup" :options="options">
        <div
          class="tile"
          v-for="tile in p2Pool"
          :key="tile.id"
          :class="isFixed(false)"
        >
          {{ tile.name }}
        </div>
      </draggable>
    </div>
    <div>
      <h3>P2の河（{{ p2River.length - 1 }}）</h3>
      <draggable
        v-model="p2River"
        group="myGroup"
        :options="options"
        @add="onP2Add"
      >
        <div
          class="tile"
          v-for="tile in p2River"
          :key="tile.id"
          :class="isFixed(true)"
        >
          {{ tile.name }}
        </div>
      </draggable>
    </div>
    <div class="dora">
      <h3>ドラ表</h3>
      <div class="tile">{{ dora }}</div>
    </div>
    <div class="dora">
      <h3>裏ドラ表</h3>
      <div class="tile">{{ uradora }}</div>
    </div>
    <div>
      <h3>P1の河（{{ p1River.length - 1 }}）</h3>
      <draggable
        v-model="p1River"
        group="myGroup"
        :options="options"
        @add="onP1Add"
      >
        <div
          class="tile"
          v-for="tile in p1River"
          :key="tile.id"
          :class="isFixed(true)"
        >
          {{ tile.name }}
        </div>
      </draggable>
    </div>
    <div>
      <h3>P1の打牌候補 （{{ p1Pool.length }}）</h3>
      <draggable v-model="p1Pool" group="myGroup" :options="options">
        <div
          class="tile"
          v-for="tile in p1Pool"
          :key="tile.id"
          :class="isFixed(false)"
        >
          {{ tile.name }}
        </div>
      </draggable>
    </div>
    <div>
      <h3>P1の手牌（{{ p1Hand.length }}）</h3>
      <draggable v-model="p1Hand" group="myGroup" :options="options">
        <div
          class="tile"
          v-for="tile in p1Hand"
          :key="tile.id"
          :class="isFixed(false)"
        >
          {{ tile.name }}
        </div>
      </draggable>
    </div>

    <div class="button">
      <button v-on:click="initialize">リセット</button>
    </div>
    <div class="button">
      <button v-on:click="p1Start">P1で開始</button>
    </div>
    <div class="button">
      <button v-on:click="p2Start">P2で開始</button>
    </div>
    <div class="button">
      <button v-on:click="win">栄和</button>
    </div>
    <div class="button">
      <button v-on:click="lose">放銃</button>
    </div>
  </div>
</template>

<script>
import draggable from "vuedraggable";
import * as firebase from "firebase/app";
import "firebase/database";
import "firebase/analytics";
//import Vue from 'vue'

const firebaseConfig = {
  apiKey: process.env.VUE_APP_API_KEY,
  authDomain: process.env.VUE_APP_PROJECT_ID + ".firebaseapp.com",
  databaseURL: "https://" + process.env.VUE_APP_PROJECT_ID + ".firebaseio.com",
  projectId: process.env.VUE_APP_PROJECT_ID,
  storageBucket: process.env.VUE_APP_PROJECT_ID + ".appspot.com",
  messagingSenderId: process.env.VUE_APP_SENDER_ID,
  appId: process.env.VUE_APP_APP_ID,
  measurementId: process.env.VUE_APP_MEASUREMENT_ID,
};
// Initialize Firebase
firebase.initializeApp(firebaseConfig);
firebase.analytics();

export default {
  name: "junanaho",

  components: { draggable },

  data() {
    return {
      options: {
        group: "myGroup",
        animation: 200,
        filter: ".fixed",
      },
      p1River: [{ id: 100, name: "->" }],
      p1Pool: [
        { id: 0, name: "??" },
        { id: 1, name: "??" },
        { id: 2, name: "??" },
        { id: 3, name: "??" },
        { id: 4, name: "??" },
        { id: 5, name: "??" },
        { id: 6, name: "??" },
        { id: 7, name: "??" },
        { id: 8, name: "??" },
        { id: 9, name: "??" },
        { id: 10, name: "??" },
        { id: 11, name: "??" },
        { id: 12, name: "??" },
        { id: 13, name: "??" },
        { id: 14, name: "??" },
        { id: 15, name: "??" },
        { id: 16, name: "??" },
        { id: 17, name: "??" },
        { id: 18, name: "??" },
        { id: 19, name: "??" },
        { id: 20, name: "??" },
      ],
      p1Hand: [
        { id: 21, name: "??" },
        { id: 22, name: "??" },
        { id: 23, name: "??" },
        { id: 24, name: "??" },
        { id: 25, name: "??" },
        { id: 26, name: "??" },
        { id: 27, name: "??" },
        { id: 28, name: "??" },
        { id: 29, name: "??" },
        { id: 30, name: "??" },
        { id: 31, name: "??" },
        { id: 32, name: "??" },
        { id: 33, name: "??" },
      ],
      p2River: [{ id: 100, name: "->" }],
      p2Pool: [
        { id: 34, name: "??" },
        { id: 35, name: "??" },
        { id: 36, name: "??" },
        { id: 37, name: "??" },
        { id: 38, name: "??" },
        { id: 39, name: "??" },
        { id: 40, name: "??" },
        { id: 41, name: "??" },
        { id: 42, name: "??" },
        { id: 43, name: "??" },
        { id: 44, name: "??" },
        { id: 45, name: "??" },
        { id: 46, name: "??" },
        { id: 47, name: "??" },
        { id: 48, name: "??" },
        { id: 49, name: "??" },
        { id: 50, name: "??" },
        { id: 51, name: "??" },
        { id: 52, name: "??" },
        { id: 53, name: "??" },
        { id: 54, name: "??" },
      ],
      p2Hand: [
        { id: 55, name: "??" },
        { id: 56, name: "??" },
        { id: 57, name: "??" },
        { id: 58, name: "??" },
        { id: 59, name: "??" },
        { id: 60, name: "??" },
        { id: 61, name: "??" },
        { id: 62, name: "??" },
        { id: 63, name: "??" },
        { id: 64, name: "??" },
        { id: 65, name: "??" },
        { id: 66, name: "??" },
        { id: 67, name: "??" },
      ],
      initialRiver: [{ id: 100, name: "->" }],
      initialPool: [
        { id: 34, name: "??" },
        { id: 35, name: "??" },
        { id: 36, name: "??" },
        { id: 37, name: "??" },
        { id: 38, name: "??" },
        { id: 39, name: "??" },
        { id: 40, name: "??" },
        { id: 41, name: "??" },
        { id: 42, name: "??" },
        { id: 43, name: "??" },
        { id: 44, name: "??" },
        { id: 45, name: "??" },
        { id: 46, name: "??" },
        { id: 47, name: "??" },
        { id: 48, name: "??" },
        { id: 49, name: "??" },
        { id: 50, name: "??" },
        { id: 51, name: "??" },
        { id: 52, name: "??" },
        { id: 53, name: "??" },
        { id: 54, name: "??" },
      ],
      initialHand: [
        { id: 55, name: "??" },
        { id: 56, name: "??" },
        { id: 57, name: "??" },
        { id: 58, name: "??" },
        { id: 59, name: "??" },
        { id: 60, name: "??" },
        { id: 61, name: "??" },
        { id: 62, name: "??" },
        { id: 63, name: "??" },
        { id: 64, name: "??" },
        { id: 65, name: "??" },
        { id: 66, name: "??" },
        { id: 67, name: "??" },
      ],
      dora: "??",
      uradora: "??",
      status: {
        moving: false,
        fixed: false,
      },
    };
  },

  computed: {
    isFixed() {
      return (fixed) => {
        return {
          fixed: fixed === true,
        };
      };
    },
  },

  methods: {
    test() {
      console.log("test");
    },
    initialize() {
      resetRef();

      const mm = ["m1", "m2", "m3", "m4", "m5", "m6", "m7", "m8", "m9"];
      const pp = ["p1", "p2", "p3", "p4", "p5", "p6", "p7", "p8", "p9"];
      const ss = ["s1", "s2", "s3", "s4", "s5", "s6", "s7", "s8", "s9"];
      const zz = ["z1", "z2", "z3", "z4", "z5", "z6", "z7"];
      var arr = [];
      for (let i = 0; i < 4; i++) {
        Array.prototype.push.apply(arr, mm);
        Array.prototype.push.apply(arr, pp);
        Array.prototype.push.apply(arr, ss);
        Array.prototype.push.apply(arr, zz);
      }
      for (let i = arr.length - 1; i > 0; i--) {
        var r = Math.floor(Math.random() * (i + 1));
        var tmp = arr[i];
        arr[i] = arr[r];
        arr[r] = tmp;
      }
      var p1Tiles = [];
      var p2Tiles = [];
      var cnt = 0;
      for (let value of arr.slice(0, 34).sort()) {
        p1Tiles.push({ id: cnt, name: value });
        cnt++;
      }
      for (let value of arr.slice(34, 68).sort()) {
        p2Tiles.push({ id: cnt, name: value });
        cnt++;
      }

      const gameRef = firebase.database().ref("/game");
      gameRef.set({
        dora: arr[68],
        uradora: arr[69],
        p1Pool: p1Tiles.slice(0, 21),
        p1Hand: p1Tiles.slice(21),
        p1River: this.initialRiver,
        p2Pool: p2Tiles.slice(0, 21),
        p2Hand: p2Tiles.slice(21),
        p2River: this.initialRiver,
      });

      console.log("initialize finish.");
    },
    p1Start() {
      resetRef();

      const gameRef = firebase.database().ref("/game");
      gameRef.once("value", (snapshot) => {
        this.dora = snapshot.val()["dora"];
        this.p1Pool = snapshot.val()["p1Pool"];
        this.p1Hand = snapshot.val()["p1Hand"];
      });

      p2Ref.on("value", (snapshot) => {
        this.p2River = snapshot.val();
        console.log("p2RiverRef");
      });

      this.p2Pool = this.initialPool;
      this.p2Hand = this.initialHand;
      this.uradora = "??";

      console.log("start as p1.");
    },
    p2Start() {
      resetRef();

      const gameRef = firebase.database().ref("/game");
      gameRef.once("value", (snapshot) => {
        this.dora = snapshot.val()["dora"];
        this.p2Pool = snapshot.val()["p2Pool"];
        this.p2Hand = snapshot.val()["p2Hand"];
      });

      const p1Ref = firebase.database().ref("/game/p1River");
      p1Ref.off();
      p1Ref.on("value", (snapshot) => {
        this.p1River = snapshot.val();
        console.log("p1RiverRef");
      });

      this.p1Pool = this.initialPool;
      this.p1Hand = this.initialHand;
      this.uradora = "??";

      console.log("start as p2.");
    },
    win() {
      resetRef();

      const gameRef = firebase.database().ref("/game");
      gameRef.update({
        p1Hand: this.p1Hand,
        p2Hand: this.p2Hand,
      });
      gameRef.once("value", (snapshot) => {
        this.uradora = snapshot.val()["uradora"];
      });
      console.log("I win.");
    },
    lose() {
      resetRef();

      const gameRef = firebase.database().ref("/game");
      gameRef.once("value", (snapshot) => {
        this.uradora = snapshot.val()["uradora"];
        this.p1Hand = snapshot.val()["p1Hand"];
        this.p2Hand = snapshot.val()["p2Hand"];
      });
      console.log("I lose.");
    },
    resetRef() {
      const p1Ref = firebase.database().ref("/game/p1River");
      p1Ref.off();
      const p2Ref = firebase.database().ref("/game/p2River");
      p2Ref.off();
    },
    // フィルタされている要素を選択した時（filterイベント）
    onFilter() {
      console.log("onFilter");
      this.status.fixed = true;
      setTimeout(() => {
        this.status.fixed = false;
      }, 1000);
    },
    onP1Add() {
      console.log("onP1Add");
      const gameRef = firebase.database().ref("/game");
      gameRef.update({
        p1River: this.p1River,
      });
    },
    onP2Add() {
      console.log("onP2Add");
      const gameRef = firebase.database().ref("/game");
      gameRef.update({
        p2River: this.p2River,
      });
    },
  },
};
</script>

<style scoped>
.dora {
  display: inline-block;
  width: 80px;
}
.button {
  display: inline-block;
}
.tile {
  display: inline-block;
  margin: 0px;
  padding: 10px;
  border: 1px solid #7f7f7f;
  border-radius: 10px;
  background-color: #ffffff;
}
.tile:hover {
  cursor: grab;
}
.tile:active {
  cursor: grabbing;
}
.sortable-chosen {
  background-color: #42b983;
}
</style>
