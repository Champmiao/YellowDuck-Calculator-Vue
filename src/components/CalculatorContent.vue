<template>
  <div class="calculator-border">
    <!-- 显示区 -->
    <div class="calculator-display">
      <div class="calculator-note" v-cloak>{{ note }}</div>
      <div class="right">
        <div class="calculator-formula" v-cloak>{{ formula }}</div>
        <div class="calculator-result" v-cloak>{{ result }}</div>
      </div>
    </div>

    <!-- 小黄鸭图标 -->
    <img class="duck" :src="imgUrl" alt />

    <!-- 输入区 -->
    <div class="calculator-items">
      <div class="calculator-row-1">
        <div class="calculator-item" @click="toggle()">±</div>
        <div class="calculator-item" @click="reciprocal()">1/x</div>
        <div class="calculator-item" @click="drop()">←</div>
        <div class="calculator-item" @click="cleanResult()">CE</div>
        <div class="calculator-item" @click="cleanAll()">C</div>
      </div>

      <div class="calculator-row-2">
        <div class="calculator-item" @click="operate(7)">7</div>
        <div class="calculator-item" @click="operate(8)">8</div>
        <div class="calculator-item" @click="operate(9)">9</div>
        <div class="calculator-item" @click="square()">√</div>
        <div class="calculator-item" @click="operate('/')">/</div>
      </div>

      <div class="calculator-row-3">
        <div class="calculator-item" @click="operate(4)">4</div>
        <div class="calculator-item" @click="operate(5)">5</div>
        <div class="calculator-item" @click="operate(6)">6</div>
        <div class="calculator-item" @click="percentage()">%</div>
        <div class="calculator-item" @click="operate('*')">x</div>
      </div>

      <div class="calculator-row-4">
        <div class="calculator-item" @click="operate(1)">1</div>
        <div class="calculator-item" @click="operate(2)">2</div>
        <div class="calculator-item" @click="operate(3)">3</div>
        <div class="calculator-item" @click="operate('.')">.</div>
        <div class="calculator-item" @click="operate('-')">-</div>
      </div>

      <div class="calculator-row-5">
        <div class="calculator-item" @click="operate(0)">0</div>
        <div class="calculator-item" @click="operate('+')">+</div>
        <div class="calculator-item" @click="equal()">=</div>
      </div>
    </div>
  </div>
</template>

<script>
import yellowduck from "../assets/yellowduck.png";
export default {
  name: "CalculatorContent",
  data() {
    return {
      formula: "",
      result: 0,
      note: "", //左侧提示
      imgUrl: require("../assets/yellowduck.png")
    };
  },

  methods: {
    // 数字或普通运算符("+ - * / %")
    operate(ele) {
      // 功能：若用户第一次按错运算符，可直接输入新运算符，无需退格或重新输入
      // 输入是否为四则运算符、百分号？
      if (ele == "+" || ele == "-" || ele == "*" || ele == "/" || ele == "%") {
        // 当前表达式末尾与输入为相同运算符？
        if (this.formula.endsWith(ele)) {
          // 输入相同运算符则去掉第一个
          this.formula = this.formula.slice(0, -1);
        } else {
          // 若输入为运算符，当前表达式末尾是数字，不做处理
          if (!isNaN(this.formula.charAt(this.formula.length - 1))) {
          } else {
            // 若输入与当前表达式末尾为不同运算符，去掉第一个运算符
            this.formula = this.formula.slice(0, -1);
          }
          // 将第二个运算符添加入表达式
          this.formula += ele;
        }
      } else {
        // 输入为数字
        this.formula += ele;
      }
    },

    // 等于
    equal() {
      try {
        // eval()函数是本计算器依赖的主要函数，该函数的作用是计算某个字符串，并执行其中的的 JavaScript 代码。
        this.result = eval(this.formula);
        // 若表达式倒数两位字符分别为"/"和"0",则特殊性提示除数不得为0
        if (
          this.formula.charAt(this.formula.length - 1) === "0" &&
          this.formula.charAt(this.formula.length - 2) === "/"
        ) {
          this.note = "除数不为0请重新输入";
          setTimeout(() => {
            this.note = "";
          }, 2000);
        }
        // 当表达式末尾为"."时，eval函数可以计算结果，但表达式有反规则，同样抛出错误提示
        else if (this.formula.endsWith(".")) {
          this.noting();
        }
        // this.result = "";
      } catch (exception) {
        this.noting();
      }
    },

    // 清除结果
    cleanResult() {
      this.result = 0;
    },

    // 清除表达式和结果
    cleanAll() {
      this.formula = "";
      this.result = 0;
    },

    // 退格
    drop() {
      console.log(this.formula.charAt(3));

      this.formula = this.formula.slice(0, -1);
    },

    // 平方根
    square() {
      try {
        // 表达式合法但为负数值时，进行特殊性提示
        if (eval(this.formula) < 0) {
          this.note = "无法对负数计算平方根";
          setTimeout(() => {
            this.note = "";
          }, 2000);
        }
        // 当表达式末尾为"."时，eval函数可以计算结果，但表达式有反规则，同样抛出错误提示
        else if (this.formula.endsWith(".")) {
          this.noting();
        } else {
          this.result = Math.sqrt(eval(this.formula));
        }
      } catch (exception) {
        this.noting();
      }
    },

    // 百分号(仅做除以100作用)
    percentage() {
      try {
        // 为了避免"%"所带来的计算优先级问题(例如当表达式为3+3,再按%不会优先计算加法运算)，计算前先对表达式使用"()"
        this.formula = "(" + this.formula + ")/100";
        this.equal();
      } catch (exception) {
        this.noting();
      }
    },

    // 倒数
    reciprocal() {
      try {
        this.formula = "1/(" + this.formula + ")";
        this.equal();
      } catch (exception) {
        this.noting();
      }
    },

    // 相反数
    toggle() {
      try {
        // 功能：为防止多次点击取反按钮，使表达式中"()"过多，多次取反前先将表达式赋值为其计算结果
        // 原理：当前表达式头部是否为"-"，若是则将表达式的结果取绝对值，若不是则为表达式两边加"()"再取反
        this.formula.startsWith("-")
          ? (this.formula = Math.abs(eval(this.formula)).toString())
          : (this.formula = "-(" + this.formula + ")");
        this.equal();
      } catch (exception) {
        this.noting();
      }
    },

    // 一般性错误提示：当eval()参数不合法无法计算时，进行提示。(其他特殊性错误提示嵌入不同的函数中)
    noting() {
      this.note = "您输入有误请重新审查";
      this.result = "";
      setTimeout(() => {
        this.note = "";
      }, 2000);
    }
  }
};
</script>


<style>
/*vue*/
[v-cloak] {
  display: none;
}

.calculator .calculator-border .calculator-display {
  border: 0.15rem solid #b9b4b4;
  background-color: #f5f8f8;
  margin: 1rem;
  /*width: 24rem;*/
  height: 7rem;
  width: 92.3%;
  font-size: 2rem;
  /*height:18.4%;*/
  -webkit-border-radius: 0.4rem;
  -moz-border-radius: 0.4rem;
  -ms-border-radius: 0.4rem;
  -o-border-radius: 0.4rem;
  border-radius: 0.4rem;
  -webkit-box-shadow: 1rem 1rem 0.5rem #b55858;
  box-shadow: 0.1rem 0.1rem 0.5rem #b55858;
  position: relative;
}

.duck {
  margin: 0.6rem auto;
  width: 35px;
  height: 35px;
}

.right {
  float: right;
  width: 70%;
  height: 100%;
}

.calculator-note {
  width: 30%;
  height: 100%;
  text-align: left;
  box-sizing: border-box;
  padding: 0.5rem 1rem;
  color: #ce3b3b;
  text-overflow: ellipsis;
  white-space: wrap;
  overflow: hidden;
  position: absolute;
  font-size: 1.4rem;
}

.calculator .calculator-border .calculator-formula {
  /*border: 1px solid red;*/
  width: 100%;
  height: 42.9%;
  text-align: right;
  box-sizing: border-box;
  padding: 0.5rem 1rem;
  color: #ce3b3b;
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
}

.calculator .calculator-border .calculator-result {
  /*border: 1px solid blue;*/
  width: 100%;
  height: 57.1%;
  text-align: right;
  box-sizing: border-box;
  padding: 1.5rem 1rem;
  color: #2a363b;
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
}

.calculator .calculator-border .calculator-items {
  /*border: 1px solid black;*/
  margin: 0.5rem auto;
  text-align: left;
  width: 92.3%;
  height: auto;
}

.calculator .calculator-border .calculator-items .calculator-row-1,
.calculator .calculator-border .calculator-items .calculator-row-2,
.calculator .calculator-border .calculator-items .calculator-row-3,
.calculator .calculator-border .calculator-items .calculator-row-4,
.calculator .calculator-border .calculator-items .calculator-row-5 {
  /*border: 1px solid blue;*/
  margin-bottom: 2.3%;
  width: 100%;
}

.calculator .calculator-border .calculator-items .calculator-item {
  border: 0.1rem solid rgba(166, 98, 98, 0.74);
  width: 16.666%;
  height: 4rem;
  display: inline-block;
  text-align: center;
  line-height: 4rem;
  margin-right: 1.8%;
  color: #2a363b;
  cursor: pointer;
  -webkit-border-radius: 0.5rem;
  -moz-border-radius: 0.5rem;
  -ms-border-radius: 0.5rem;
  -o-border-radius: 0.5rem;
  border-radius: 0.5rem;
  background-color: rgba(244, 135, 135, 0.99);
  -webkit-box-shadow: 0.1rem 0.1rem 0.3rem rgb(85, 65, 65);
  box-shadow: 0.1rem 0.1rem 0.3rem rgb(85, 65, 65);
  transition: all 0.1s;
  -webkit-transition: all 0.1s;
}

.calculator .calculator-border .calculator-items .calculator-item:hover {
  color: #f1f5f7;
  -webkit-box-shadow: 0.3rem 0.3rem 0.5rem #b55858;
  box-shadow: 0.3rem 0.3rem 0.5rem #b55858;
}

.calculator .calculator-border .calculator-items .calculator-item:active {
  transform: translate(0.05rem, 0.05rem);
  -moz-transform: translate(0.05rem, 0.05rem);
  -webkit-transform: translate(0.05rem, 0.05rem);
}

.calculator
  .calculator-border
  .calculator-row-5
  .calculator-item:nth-child(n + 2) {
  width: 37.1%;
}

.calculator .calculator-border .calculator-row-1 .calculator-item:nth-child(5),
.calculator .calculator-border .calculator-row-2 .calculator-item:nth-child(5),
.calculator .calculator-border .calculator-row-3 .calculator-item:nth-child(5),
.calculator .calculator-border .calculator-row-4 .calculator-item:nth-child(5),
.calculator .calculator-border .calculator-row-5 .calculator-item:nth-child(3) {
  margin-right: 0;
}
</style>