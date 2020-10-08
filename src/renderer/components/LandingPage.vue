<template>
 <div class="container">
  <div class="calc-body">
    <div class="calc-screen">
      <div :class="equal=='value' ? 'calc-operation equal' : 'calc-operation'">{{value}}</div>
      <div :class="equal=='result' ? 'calc-typed equal' : 'calc-typed'">{{result}}
        <transition name="fade">
        <span v-if="togle" class="blink-me">_</span>
        </transition>
        </div>
    </div>
    <div class="calc-button-row">
      <div v-for="item in buttons[0]" :key="item" :class="item=='C' ? 'button c' : 'button l'" @click="buttonClick(item)">{{item}}</div>
      <!-- <div class="button c">C</div>
      <div class="button l">≠</div>
      <div class="button l">%</div>
      <div class="button l">/</div> -->
    </div>
    <div class="calc-button-row">
      <div  v-for="item in buttons[1]" :key="item" :class="item=='x' ? 'button l' : 'button'" @click="buttonClick(item)">{{item}}</div>
      <!-- <div class="button">7</div>
      <div class="button">8</div>
      <div class="button">9</div>
      <div class="button l">x</div> -->
    </div>
    <div class="calc-button-row">
      <div  v-for="item in buttons[2]" :key="item" :class="item!='-' ? 'button' : 'button l'" @click="buttonClick(item)">{{item}}</div>
      <!-- <div class="button">4</div>
      <div class="button">5</div>
      <div class="button">6</div>
      <div class="button l">−</div> -->
    </div>
    <div class="calc-button-row">
      <div  v-for="item in buttons[3]" :key="item" :class="item!='+' ? 'button' : 'button l'" @click="buttonClick(item)">{{item}}</div>

      <!-- <div class="button">1</div>
      <div class="button">2</div>
      <div class="button">3</div>
      <div class="button l">+</div> -->
    </div>
    <div class="calc-button-row">
      <div  v-for="item in buttons[4]" :key="item" :class="item!='=' ? 'button' : 'button l'" @click="buttonClick(item)">{{item}}</div>
      <!-- <div class="button">.</div>
      <div class="button">0</div>
      <div class="button">
        <</div>
          <div class="button l">=</div> -->
      </div>
    </div>
  </div>
</template>

<script>
  import SystemInformation from './LandingPage/SystemInformation'

  export default {
    name: 'landing-page',
    components: { SystemInformation },
    data: () => ({
      togle: '',
      equal:'value',
      buttons: [
        ['C','≠','%','/'],
        [7,8,9,'x'],
        [4,5,6,'-'],
        [1,2,3,'+'],
        ['.',0,'<','=']
        ],
        value:'',
        result:null
    }),

    methods: {
      open (link) {
        this.$electron.shell.openExternal(link)
      },
      buttonClick(button){
        let arr=['+','-','/','x','%','.']
        if(arr.includes(this.value[this.value.length-1]) && arr.includes(button)){
          return
        }
        let splitArr=this.value.split(/[-,+,x,/,%]/g)
        if(splitArr[splitArr.length-1].indexOf('.')==1 && button=="."){
          return
       }
        this.value+=button
        let res=this.value
        res=res.replace(/x/g,"*");
        switch(button){
          case "+":
            break;
          case "-":
            break;
          case "%":
            break;
          case "/":
            break;
          case "x":
            break;
          case "C":
            this.result = "";
            this.value = "";
            break;
          case "=":
            this.equal='result';
            break;
          case ".":

            break;
          default:
            console.log(res)
          this.result=eval(res)

        }
      }
    },
    mounted () {
      setInterval(() => {
        setTimeout(() => {
          this.togle = false
        }, 500)
        this.togle = true
      }, 500)
    }
  }
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=Source+Sans+Pro');
* {
  box-sizing: border-box;
  font-family: 'Roboto', sans-serif;
}

body {
  background: #EAEBEC;
}

.container {
  width: 400px;
  margin: auto;
}
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active до версии 2.1.8 */ {
  opacity: 0;
}
.calc-body {
  width: 275px;
  margin: auto;
  min-height: 400px;
  border: solid 1px #3A4655;
  box-shadow: 0 8px 50px -7px black;
}

.calc-screen {
  background: #3A4655;
  width: 100%;
  height: 150px;
  padding: 20px;
}
.equal{
  color: #fff !important;

}

.calc-operation {
  color: #727B86;
  text-align: right;
  font-size: 21px;
  padding-bottom: 10px;
  border-bottom: dotted 1px;
}

.calc-typed {
  color: #727B86;
  margin-top: 20px;
  font-size: 45px;
  text-align: right;
}

.calc-button-row {
  width: 100%;
  background: #3C4857;
}

.button {
  width: 25%;
  background: #425062;
  color: #fff;
  padding: 20px;
  display: inline-block;
  font-size: 25px;
  text-align: center;
  vertical-align: middle;
  /* margin-right: -4px; */
  border-right: solid 2px #3C4857;
  border-bottom: solid 2px #3C4857;
  transition: all 0.2s ease-in-out;
}

.button.l {
  color: #AEB3BA;
  background: #404D5E;
}

.button.c {
  color: #D95D4E;
  background: #404D5E;
}

.button:hover {
  background: #E0B612;
  transform: rotate(5deg);
}

.button.c:hover,
.button.l:hover {
  background: #E0B612;
  color: #fff;
}

.blink-me {
  opacity: 1;
  color: #E0B612;
}
</style>