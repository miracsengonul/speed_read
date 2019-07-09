<template>
  <div id="app">
    <div class="container">
      <div class="row message">
        <span v-for="(word,index) in sentence">
          <span v-if="range==index">
            <span class="messageText">{{word}}</span>
          </span>
        </span>
      </div>
      <div class="row range">
        <input type="range" class="form-control" value="0" :max="sentence.length - 1" v-model="range" />
      </div>
      <div class="row mainRow">
        <div class="col-md" v-show="range == 0">
          <button type="text" class="button btn btn-lg btn-success" @click="start">Başla</button>
          <h2 class="speedHead"><b>- Hız -</b></h2>
          <input type="number" class="speedInput" v-model="speed">
        </div>
        <div class="col-md" v-show="range < sentence.length - 1 && range != 0">
          <button type="text" class="button btn btn-lg btn-info" @click="pause" v-show="isWalking" >Dur</button>
          <button type="text" class="button btn btn-lg btn-warning" @click="play" v-show="!isWalking" >Devam Et</button>
        </div>
        <div class="col-md" v-show="range == sentence.length - 1">
          <button type="text" class="button btn btn-lg btn-primary" @click="restart">Tekrar</button>
        </div>
        <div class="col-md" v-show="range == sentence.length - 1 || isNewText && range != 0">
          <button type="text" class="button btn btn-lg btn-dark" @click="newText">Yeni Metin</button>
        </div>
      </div>
      <div class="row textareaDiv" v-if="textarea">
        <h2 class="textareaHead"><b>- Metin -</b></h2>
        <textarea class="form-control" v-model="message"></textarea>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      range: 0,
      isWalking: true,
      isNewText: false,
      textarea: true,
      speed: 245,
      message: "Deneme mesaj içeriği."
    };
  },
  computed: {
    sentence() {
      return this.message.split(" ");
    }
  },
  methods: {
    start(speed) {
      let vm = this;
      let range = this.range;
      this.isWalking = true;
      let messageCounter = this.sentence.length - 2;
      this.textarea = false;

      let setFunc = setInterval(function() {
        if (vm.range <= messageCounter && vm.isWalking) {
          vm.range++;
        } else {
          clearInterval(setFunc);
        }
      }, (this.speed / (this.speed-6)).toFixed(4).slice(-3));
    },
    restart() {
      this.range = 0;
      this.start();
    },
    pause() {
      this.range = this.range;
      this.isWalking = false;
      this.isNewText = true;
    },
    play() {
      this.range = this.range;
      this.isWalking = true;
      this.start();
    },
    newText() {
      this.range = 0;
      this.isWalking = false;
      this.textarea = true;
    }
  }
};
</script>

<style>
  .button{box-shadow: 2px 2px 2px #333;}
  .speedInput{width:150px;height:50px;text-align:center;font-size:30px;}
  .mainRow{text-align:center;margin-top:50px;}
  .message{margin-left:auto;margin-right:auto;display:block;text-align:center;color:black;margin-top:70px}
  .messageText{font-size:125px;}
  .textareaDiv{margin-top:40px;margin-bottom:50px;}
  .textareaHead{margin-left:auto;margin-right:auto;display:block;text-align:center}
  .speedHead{margin-top:35px;}
  .range{margin-top:50px}
</style>
