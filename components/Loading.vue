<template>
    <div align="center" v-if="loading" class="loading-page">
        <div class="load-wrapp">
            <div class="load-1">
                <div class="line"></div>
                <div class="line"></div>
                <div class="line"></div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  data: () => ({
    loading: false
  }),
  methods: {
    start() {
      this.loading = true
    },
    finish() {
      this.loading = false
    }
  }
}
</script>

<style>
.loading-page {
    position: absolute;
    background-color: rgba(81, 81, 81, 0.666);
    width: 100%;
    z-index: 999;
    padding-bottom: 700px;
    padding-top: 300px;
}

.load-wrapp {
    width: 100px;
    height: 100px;
    margin: 0 10px 10px 0;
    padding: 20px 20px 20px;
    border-radius: 5px;
    background-color: #d8d8d8;
}

.load-wrapp:last-child {margin-right: 0;}

.line {
    display: inline-block;
    width: 15px;
    height: 15px;
    border-radius: 15px;
    background-color: #4b9cdb;
}

.load-1 .line:nth-last-child(1) {animation: loadingA 1.5s 1s infinite;}
.load-1 .line:nth-last-child(2) {animation: loadingA 1.5s .5s infinite;}
.load-1 .line:nth-last-child(3) {animation: loadingA 1.5s 0s infinite;}

.load-2 .line:nth-last-child(1) {animation: loadingB 1.5s 1s infinite;}
.load-2 .line:nth-last-child(2) {animation: loadingB 1.5s .5s infinite;}
.load-2 .line:nth-last-child(3) {animation: loadingB 1.5s 0s infinite;}

.load-3 .line:nth-last-child(1) {animation: loadingC .6s .1s linear infinite;}
.load-3 .line:nth-last-child(2) {animation: loadingC .6s .2s linear infinite;}
.load-3 .line:nth-last-child(3) {animation: loadingC .6s .3s linear infinite;}

.l-1 {animation-delay: .48s;}
.l-2 {animation-delay: .6s;}
.l-3 {animation-delay: .72s;}
.l-4 {animation-delay: .84s;}
.l-5 {animation-delay: .96s;}
.l-6 {animation-delay: 1.08s;}
.l-7 {animation-delay: 1.2s;}
.l-8 {animation-delay: 1.32s;}
.l-9 {animation-delay: 1.44s;}
.l-10 {animation-delay: 1.56s;}

.load-8 .line {animation: loadingH 1.5s cubic-bezier(.17,.37,.43,.67) infinite;}

.load-10 .bar {animation: loadingJ 2s cubic-bezier(.17,.37,.43,.67) infinite;}

@keyframes loadingA {
    0% {height: 15px;}
    50% {height: 35px;}
    100% {height: 15px;}
}

@keyframes loadingB {
    0% {width: 15px;}
    50% {width: 35px;}
    100% {width: 15px;}
}

@keyframes loadingC {
    0% {transform: translate(0,0);}
    50% {transform: translate(0,15px);}
    100% {transform: translate(0,0);}
}

@keyframes loadingD {
    0% {transform: rotate(0deg);}
    50% {transform: rotate(180deg);}
    100% {transform: rotate(360deg);}
}

@keyframes loadingE {
    0% {transform: rotate(0deg);}
    100% {transform: rotate(360deg);}
}

@keyframes loadingF {
    0% {opacity: 0;}
    100% {opacity: 1;}
}

@keyframes loadingG {
    0% {transform: translate(0,0) rotate(0deg);}
    50% {transform: translate(70px,0) rotate(360deg);}
    100% {transform: translate(0,0) rotate(0deg);}
}

@keyframes loadingH {
    0% {width: 15px;}
    50% {width: 35px; padding: 4px;}
    100% {width: 15px;}
}

@keyframes loadingI {
    100% {transform: rotate(360deg);}
}

@keyframes bounce  {
  0%, 100% {transform: scale(0.0);}
  50% {transform: scale(1.0);}
}

@keyframes loadingJ {
  0%,100% {transform: translate(0,0);}
  
  50% {
      transform: translate(80px,0);
      background-color: #f5634a;
      width: 25px;
  }
}
</style>
