<template src="./template.html"></template>
<style scoped src="./style.css"></style>

<script>
import draggable from "vuedraggable";
import * as firebase from "firebase/app";
import "firebase/database";
import "firebase/analytics";

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
firebase.initializeApp(firebaseConfig);
firebase.analytics();

export default {
  name: "junanaho",

  components: { draggable },

  data() {
    return {
      lobby: true,
      options: {
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
      id: "",
      roomId: "",
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
    createId() {
      return String(Math.random()).substr(2, 4);
    },
    createRoom() {
      this.roomId = this.createId();
      this.id = this.roomId;
      this.lobby = false;
      this.initialize();
      this.p1Start();
    },
    joinRoom() {
      if (this.roomId == "") {
        alert("ルームがありません");
        return;
      }
      const gameRef = firebase.database().ref("/game/" + this.roomId);
      gameRef.once("value", (snapshot) => {
        if (!snapshot.val()) {
          alert("ルームがありません");
          return;
        }
      });

      do {
        this.id = this.createId();
      } while (this.id == this.roomId);

      this.lobby = false;
      this.p2Start();
    },
    initialize() {
      this.listernerOff();

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

      const gameRef = firebase.database().ref("/game/" + this.roomId);
      gameRef.set({
        dora: arr[68],
        uradora: arr[69],
        p1Pool: p1Tiles.slice(0, 21),
        p1Hand: p1Tiles.slice(21),
        p1River: this.initialRiver,
        p2Pool: p2Tiles.slice(0, 21),
        p2Hand: p2Tiles.slice(21),
        p2River: this.initialRiver,
        status: 0,
      });

      console.log("initialize finish.");
    },
    p1Start() {
      this.listernerOff();

      const gameRef = firebase.database().ref("/game/" + this.roomId);
      gameRef.once("value", (snapshot) => {
        this.dora = snapshot.val()["dora"];
        this.p1Pool = snapshot.val()["p1Pool"];
        this.p1Hand = snapshot.val()["p1Hand"];
      });

      const p2Ref = firebase
        .database()
        .ref("/game/" + this.roomId + "/p2River");
      p2Ref.on("value", (snapshot) => {
        this.p2River = snapshot.val();
        console.log("p2RiverRef");
      });
      const statusRef = firebase
        .database()
        .ref("/game/" + this.roomId + "/status");
      statusRef.on("value", (snapshot) => {
        if (snapshot.val() == 1) {
          this.lose();
        }
      });

      this.p2Pool = this.initialPool;
      this.p2Hand = this.initialHand;
      this.p1River = this.initialRiver;
      this.p2River = this.initialRiver;
      this.uradora = "??";

      console.log("start as p1.");
    },
    p2Start() {
      this.listernerOff();

      const gameRef = firebase.database().ref("/game/" + this.roomId);
      gameRef.once("value", (snapshot) => {
        this.dora = snapshot.val()["dora"];
        this.p2Pool = snapshot.val()["p2Pool"];
        this.p2Hand = snapshot.val()["p2Hand"];
      });

      const p1Ref = firebase
        .database()
        .ref("/game/" + this.roomId + "/p1River");
      p1Ref.on("value", (snapshot) => {
        this.p1River = snapshot.val();
        console.log("p1RiverRef");
      });
      const statusRef = firebase
        .database()
        .ref("/game/" + this.roomId + "/status");
      statusRef.on("value", (snapshot) => {
        if (snapshot.val() == 1) {
          this.lose();
        }
      });

      this.p1Pool = this.initialPool;
      this.p1Hand = this.initialHand;
      this.p1River = this.initialRiver;
      this.p2River = this.initialRiver;
      this.uradora = "??";

      console.log("start as p2.");
    },
    win() {
      this.listernerOff();

      const gameRef = firebase.database().ref("/game/" + this.roomId);
      gameRef.update({
        p1Hand: this.p1Hand,
        p2Hand: this.p2Hand,
        status: 1,
      });
      gameRef.once("value", (snapshot) => {
        this.uradora = snapshot.val()["uradora"];
      });
      console.log("I win.");
    },
    lose() {
      this.listernerOff();

      const gameRef = firebase.database().ref("/game/" + this.roomId);
      gameRef.once("value", (snapshot) => {
        this.uradora = snapshot.val()["uradora"];
        this.p1Hand = snapshot.val()["p1Hand"];
        this.p2Hand = snapshot.val()["p2Hand"];
      });
      console.log("I lose.");
    },
    sorting() {
      this.p1Pool.sort(this.compareFunc);
      this.p1Hand.sort(this.compareFunc);
      this.p2Pool.sort(this.compareFunc);
      this.p2Hand.sort(this.compareFunc);
      console.log("sorting finish.");
    },
    compareFunc(a, b) {
      if (a.name < b.name) return -1;
      if (a.name > b.name) return 1;
      return 0;
    },
    listernerOff() {
      const p1Ref = firebase
        .database()
        .ref("/game/" + this.roomId + "/p1River");
      p1Ref.off();
      const p2Ref = firebase
        .database()
        .ref("/game/" + this.roomId + "/p2River");
      p2Ref.off();
      const statusRef = firebase
        .database()
        .ref("/game/" + this.roomId + "/status");
      statusRef.off();
    },
    onP1Add() {
      console.log("onP1Add");
      const gameRef = firebase.database().ref("/game/" + this.roomId);
      gameRef.update({
        p1River: this.p1River,
      });
    },
    onP2Add() {
      console.log("onP2Add");
      const gameRef = firebase.database().ref("/game/" + this.roomId);
      gameRef.update({
        p2River: this.p2River,
      });
    },
    parseImg(name) {
      if (name == "??") {
        name = "tile";
      }
      if (name == "->") {
        name = "je";
      }
      return require("../assets/img/" + name + ".png");
    },
  },
};
</script>
