<template>
  <div>
    <p class="type"><img src="@/assets/fat.png" />多めに払う人</p>
    <div>
      <input type="number" min="1" v-model="ome_people_num" />
    </div>
    <p class="type"><img src="@/assets/ojigi.png" />少なめに払う人</p>
    <div>
      <input type="number" min="1" v-model="sukuname_people_num" />
    </div>
    <p class="type"><img src="@/assets/yen.png" />合計金額</p>
    <div>
      <input type="number" min="1" v-model="num" />
    </div>
    <a class="btn btn-malformation calc-btn" @click="doAction">計算する！</a>
    <div
      v-if="calc && num > 0 && ome_people_num > 0 && sukuname_people_num > 0"
      class="text_exam"
    >
      <div>
        <a class="btn btn-flat">
          <span>多めに払う人</span>
        </a>
        <span>
          1人あたり:
          <span class="text_yen">{{ ome_exam | addComma }}円</span>
        </span>
      </div>
      <div>
        <a class="btn btn-flat2">
          <span>少なめに払う人</span>
        </a>
        <span>
          1人あたり:
          <span class="text_yen">{{ sukuname_exam | addComma }}円</span>
        </span>
      </div>
      <div class="flex-box">
        <div>
          <a class="btn btn-flat3">
            <span>支払金額</span>
          </a>
          <p>
            <span class="text_yen">{{ pay_exam | addComma }}円</span>
          </p>
        </div>
        <div>
          <a class="btn btn-flat4">
            <span>おつり</span>
          </a>
          <p>
            <span class="text_yen">{{ change_exam | addComma }}円</span>
          </p>
        </div>
      </div>
    </div>
    <p>© 2022 cocoa-engineer</p>
  </div>
</template>

<script>
export default {
  name: "Calculation",
  data() {
    return {
      num: "",
      ome_people_num: 1,
      sukuname_people_num: 1,
      calc: false,
    };
  },
  computed: {
    sukuname_exam() {
      const exam = Math.floor(this.num / this.all_people_num / 100);
      if (exam * 100 < 0) {
        return exam * 0;
      } else {
        return exam * 100 * 0.8;
      }
    },
    ome_exam_ceil() {
      const exam = this.num - this.sukuname_exam * this.sukuname_people_num;
      if (exam < 1000) {
        return exam;
      } else {
        return Math.ceil(exam / this.ome_people_num / 100) * 100 * 1.2;
      }
    },
    ome_exam() {
      if (1000 <= this.change_exam_before) {
        const exam = Math.floor(
          (this.ome_exam_ceil * this.ome_people_num - this.change_exam_before) /
            this.ome_people_num
        );
        return Math.floor(exam / 100) * 100;
      } else {
        return this.ome_exam_ceil;
      }
    },
    pay_exam() {
      const exam =
        this.ome_exam * this.ome_people_num +
        this.sukuname_exam * this.sukuname_people_num;
      return exam;
    },
    change_exam() {
      const exam = this.num - this.pay_exam;
      return Math.abs(exam);
    },
    pay_exam_before() {
      const exam =
        this.ome_exam_ceil * this.ome_people_num +
        this.sukuname_exam * this.sukuname_people_num;
      return exam;
    },
    change_exam_before() {
      const exam = this.num - this.pay_exam_before;
      return Math.abs(exam);
    },
    all_people_num() {
      return parseInt(this.ome_people_num) + parseInt(this.sukuname_people_num);
    },
  },
  methods: {
    doAction() {
      this.calc = true;
    },
  },
  filters: {
    addComma: function(value) {
      return value.toLocaleString();
    },
  },
};
</script>

<style scoped>
.type {
  margin-top: 3px;
  margin-bottom: 1px;
}

.coution_num {
  color: red;
  font-size: 10pt;
}
.coution_people {
  color: red;
  font-size: 10pt;
}

.text_exam {
  width: 40%;
  margin: 0 auto;
  padding: 20px 0;
  border: 3px solid #000;
  border-radius: 0.5rem;
}

.flex-box {
  display: flex;
  justify-content: center;
}

.text_yen {
  font-size: 1.5em;
  text-shadow: 1px 1px 0 rgba(0, 0, 0, 0.2);
  font-weight: bold;
  background: linear-gradient(transparent 40%, #ffff66 40%);
}

/* 計算するボタン */
*,
*:before,
*:after {
  -webkit-box-sizing: inherit;
  box-sizing: inherit;
}

html {
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
  font-size: 62.5%;
}

.calc-btn {
  font-size: 1.5rem;
}

.btn,
a.btn,
button.btn {
  margin: 15px;
  font-weight: 700;
  line-height: 1.5;
  position: relative;
  display: inline-block;
  width: 150px;
  padding: 1rem 0;
  cursor: pointer;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  -webkit-transition: all 0.3s;
  transition: all 0.3s;
  text-align: center;
  vertical-align: middle;
  text-decoration: none;
  letter-spacing: 0.1em;
  color: #212529;
  border-radius: 0.5rem;
}

a.btn-malformation {
  padding: 0.9rem 0.9rem;
  margin: 30px;

  color: #fff;
  border-radius: 100% 80px / 80px 100%;
  background-color: #eb6100;
}

a.btn-malformation:hover {
  color: #fff;
  border-radius: 60% 80% / 100% 80%;
}

/* 多めに払う人 */
a.btn-flat {
  overflow: hidden;
  color: #fff;
  background: #212529;
}

a.btn-flat span {
  position: relative;
}

a.btn-flat:before {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  content: "";
  -webkit-transition: all 0.5s ease-in-out;
  transition: all 0.5s ease-in-out;
  -webkit-transform: translateX(-96%);
  transform: translateX(-96%);
  background: #78b9ff;
}

a.btn-flat:hover:before {
  -webkit-transform: translateX(0%);
  transform: translateX(0%);
}

/* 少なめに払う人 */
a.btn-flat2 {
  overflow: hidden;
  color: #fff;
  background: #212529;
}

a.btn-flat2 span {
  position: relative;
}

a.btn-flat2:before {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  content: "";
  -webkit-transition: all 0.5s ease-in-out;
  transition: all 0.5s ease-in-out;
  -webkit-transform: translateX(-96%);
  transform: translateX(-96%);
  background: #ea6264;
}

a.btn-flat2:hover:before {
  -webkit-transform: translateX(0%);
  transform: translateX(0%);
}

/* 支払い金額 */
a.btn-flat3 {
  overflow: hidden;
  color: #fff;
  background: #212529;
}

a.btn-flat3 span {
  position: relative;
}

a.btn-flat3:before {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  content: "";
  -webkit-transition: all 0.5s ease-in-out;
  transition: all 0.5s ease-in-out;
  -webkit-transform: translateX(-96%);
  transform: translateX(-96%);
  background: #00b7ee;
}

a.btn-flat3:hover:before {
  -webkit-transform: translateX(0%);
  transform: translateX(0%);
}

/* おつり */
a.btn-flat4 {
  overflow: hidden;
  color: #fff;
  background: #212529;
}

a.btn-flat4 span {
  position: relative;
}

a.btn-flat4:before {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  content: "";
  -webkit-transition: all 0.5s ease-in-out;
  transition: all 0.5s ease-in-out;
  -webkit-transform: translateX(-96%);
  transform: translateX(-96%);
  background: #fcaa00;
}

a.btn-flat4:hover:before {
  -webkit-transform: translateX(0%);
  transform: translateX(0%);
}

input[type="number"] {
  font-size: 16px;
  transform: scale(0.8);
}

@media screen and (max-width: 480px) {
  /* 480px以下に適用されるCSS（スマホ用） */
  .text_exam {
    width: 100%;
    padding: 0;
  }

  a.btn-malformation {
    padding: 0.7rem 0.7rem;
    margin: 18px;
  }

  .text_yen {
    font-size: 1.4em;
  }

  .calc-btn {
    font-size: 1.3rem;
  }
}
</style>
