<template>
  <div id="vue-carousel-my-example" :style="{ height: stringImgHeight }">
    <link href="https://unpkg.com/animate.css@3.5.1/animate.min.css" rel="stylesheet" type="text/css">
    <div>
      <div v-for="(item,index) in img" style="float: left">
        <transition
          name="custom-classes-transition"
          :enter-active-class="enterActiveClass"
          :leave-active-class="leaveActiveClass">
          <img :src="item" v-show="currentCactiveImg === index" style="position: absolute;left:0px;top:0px" :width="stringImgWidth" :height="stringImgHeight">
        </transition>
      </div>
      <div style="position: absolute;z-index:99999999">
        <div @click="pre" :style="stringButtonStyle.pre" class="button_static"><img :src="button.left" style="width: inherit;height: inherit;"></div>
        <div @click="next" :style="stringButtonStyle.next" class="button_static"><img :src="button.right" style="width: inherit;height: inherit;"></div>
      </div>
      <ul class="vue-carousel-ul" :style="stringUlStyle">
        <li v-for="(item,index) in img" @click="chooseImg($event,index)" :style="ul"></li>
      </ul>
    </div>
  </div>
</template>

<script>
  import picture1 from './assets/picture1.jpg'
  import picture2 from './assets/picture2.jpeg'
  import picture3 from './assets/picture3.jpeg'
  import right from './assets/right.png'
  import left from './assets/left.png'
  export default {
    data () {
      return {
        currentCactiveImg: null,
        img: [picture1, picture2, picture3],
        button: {
          left: left,
          right: right
        },
        ul: {
        },
        timeOut: null,
        imgWidth: 0,
        imgHeight: 0,
        enterActiveClass: 'animated bounceInRight',
        leaveActiveClass: 'animated bounceOutLeft',
        currentDiv: null
      }
    },
    computed: {
      stringImgWidth () {
        return this.imgWidth + 'px'
      },
      stringImgHeight () {
        return this.imgHeight + 'px'
      },
      stringButtonStyle () {
        return {
          pre: {
            'left': '0px',
            'top': this.imgHeight / 2 - 50 + 'px'
          },
          next: {
            'left': this.imgWidth - 75 + 'px',
            'top': this.imgHeight / 2 - 50 + 'px'
          }
        }
      },
      stringUlStyle () {
        return {
          'top': this.imgHeight * 0.8 + 'px'
        }
      }
    },
    methods: {
      pre () {
        this.enterActiveClass = 'animated bounceInLeft'
        this.leaveActiveClass = 'animated bounceOutRight'
        this.checkPageSize(-1)
      },
      next () {
        this.enterActiveClass = 'animated bounceInRight'
        this.leaveActiveClass = 'animated bounceOutLeft'
        this.checkPageSize(1)
      },
      autoChangePage () {
        this.enterActiveClass = 'animated bounceInRight'
        this.leaveActiveClass = 'animated bounceOutLeft'
        this.checkPageSize(1)
      },
      checkPageSize (index) {
        var temp = this.currentCactiveImg + index
        if (temp > 0) {
          if (temp <= this.img.length - 1) {
            this.currentCactiveImg = temp
          } else {
            this.currentCactiveImg = 0
          }
        } else {
          this.currentCactiveImg = this.img.length - 1
        }
      },
      divOnFocus () {
        clearInterval(this.timeOut)
        this.timeOut = null
      },
      divOnBlur () {
        this.timeOut = setInterval(this.autoChangePage, 5000)
      },
      windowResize () {
        this.imgWidth = window.innerWidth + 'px'
      },
      chooseImg (li, index) {
        this.currentCactiveImg = index
        let temp = li.target.parentElement.childNodes
        console.log(temp)
        temp.forEach(function (value) {
          value.style = ''
        })
        li.target.style = 'opacity: 1;background-color: #fff;'
      }
    },
    created () {
      this.divOnBlur()
      this.imgWidth = window.innerWidth
      this.imgHeight = 300
      window.onresize = this.windowResize
      this.currentCactiveImg = 0
    },
    mounted () {
      document.querySelector('#vue-carousel-my-example').onmouseover = this.divOnFocus
      document.querySelector('#vue-carousel-my-example').onmouseout = this.divOnBlur
    },
    watch: {
      currentCactiveImg () {
        let temp = document.querySelector('.vue-carousel-ul').childNodes
        temp.forEach(function (value) {
          value.style = ''
        })
        temp[this.currentCactiveImg].style = 'opacity: 1;background-color: #fff;'
      }
    }
  }
</script>
<style>
  body {
    overflow:hidden
  }
  .button_static {
    position: absolute;
    width: 60px;
    height: 60px;
    background: #777777;
    opacity: 0.8;
    border-radius: 10px;
  }
  .button_static:hover {
    position: absolute;
    width: 60px;
    height: 60px;
    background: #999999;
    opacity: 0.8;
    border-radius: 10px;
  }
  .vue-carousel-ul {
    position: absolute;
    z-index:99999999;
    left: 50%;
    transform: translateX(-50%);
  }
  .vue-carousel-ul li{
    position: relative;
    display: inline-block;
    border-radius: 50%;
    width: 28px;
    height: 28px;
    border: 2px solid #fff;
    cursor: pointer;
    z-index:99999999;
  }

  .vue-carousel-ul li:hover{
    opacity: 1;
    background-color: #fff;
  }
</style>
