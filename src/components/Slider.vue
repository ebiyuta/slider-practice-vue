<template>
  <div class="js_slideWrapper">
    <button @click="prev" class="js_slideButton js_slideButton__prev"></button>
    <ul class="js_slider">
      <li :class="(current_slide == index)? 'js_slider_item is_active' : 'js_slider_item'" v-for="(slide, index) in slides" :key="index">
        <a href="https://ebifry.jp/" target="_blank" rel="noopener">
          <img :src='slide.img' alt="">
        </a>
      </li>
    </ul>
    <button @click="next" class="js_slideButton"></button>
    <ul class="js_slideDots">
      <li :class="(current_slide == index)? 'js_slideDots_item is_activeDots' : 'js_slideDots_item'" v-for="(slide, index) in slides" :key="index"><button @click="slideOperationDots(index)"></button></li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'Slider',
  data () {
    return {
      intervalID: '',
      trans_name: 'fade',
      current_slide: 0,
      slides: [{
        img: require('../assets/1.jpg')
      },{
        img: require('../assets/2.jpg')
      },{
        img: require('../assets/3.jpg')
      },{
        img: require('../assets/4.jpg')
      },{
        img: require('../assets/5.jpg')
      }]
    }
  },
  methods: {
    next () {
      this.current_slide++;
      if(this.current_slide == this.slides.length){
        this.current_slide = 0;
      }
    },
    prev () {
      this.current_slide--;
      if(this.current_slide == -1){
        this.current_slide = this.slides.length -1;
      }
    },
    slideOperationDots (index) {
      this.current_slide = index
    },
    autoPlay () {
      setInterval(() => {
        this.next()
      }, 5000);
    },
    restInterval () {
      clearInterval(this.intervalID);
      this.autoPlay();
    }
  },
  mounted () {
    this.autoPlay()
  }
}
</script>

<style scoped lang="scss">
.js_slider{
    margin-right: auto;
    margin-left: auto;
    padding-top: 35.2%;
    position: relative;
    &_item {
      width: 100%;
      height: 100%;
      position: absolute;
      left: 0;
      top: 0;
      opacity: 0;
      &.is_active{
        opacity: 1;
        animation: fadeIn 2s ease 0s 1 normal;
      }
    }
}

@keyframes fadeIn {
  0% {
      opacity: 0.2
  }

  100% {
      opacity: 1
  }
}

// arrowsのスタイル
.js_slideWrapper{
  position: relative;

  .js_slideButton{
    $wrp:'.js_slideButton';
    position: absolute;
    z-index: 999;
    top: 50%;
    right: 10px;
    transform: translateY(-50%);
    background-color: #fff;
    width: 50px;
    height: 50px;
    border-radius: 50%;

    &::after{
      content: '';
      display: block;
      width: 15px;
      height: 15px;
      border-top: 1px solid #000;
      border-right: 1px solid #000;
      position: absolute;
      top: 50%;
      left: calc(50% - 3px);
      transform: translateY(-50%) translateX(-50%) rotate(45deg);
    }

    &:hover{
      opacity: .7;
    }

    &__prev{
      left: 10px;

      &::after{
        left: calc(50% + 3px);
        transform: translateY(-50%) translateX(-50%) rotate(-135deg);
      }
    }
  }
}

// dotsのスタイル
.js_slideDots{
  text-align: center;
  margin-top: 10px;

  &_item{
    display: inline-block;
    margin-right: 10px;

    &:last-child{
      margin-right: 0;
    }

    > button {
      width: 10px;
      height: 10px;
      border: 1px solid #333;
      border-radius: 50%;
    }

    &.is_activeDots{
      > button {
        background-color: #333; 
      }
    }
  }
}

/* -------------------------------- */
/* 以下 Vue.js で使用するクラスの定義 */
/* -------------------------------- */
.fade-enter-active, .fade-leave-active{
  transition: all .8s ease-out;
}
.fade-enter {
  opacity: 0;
}
.fade-enter-to {
  opacity: 1;
}
.fade-leave {
  opacity: 0;
}
.fade-leave-to {
  opacity: 1;
}
</style>
