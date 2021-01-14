<template>
<div class='big_screen'>
    <div class="wrapper_img"  :style="{backgroundImage:`url(${bg_src})`}">
      <div class='content'  :style="{width:Width - 0.5+'px',height:Height - 0.5+'px'}">  
            <div class='content_inter' :style="{transform:'scale('+scaleX+','+scaleY+')',transformOrigin:'0px 0px'}"> 
                <div style='position:relative;top:0;left:0'>
                <headers style='position:absolute;left:0px;top:10px;width:100%;'></headers>
                    <!-- 左2 -->
                    <leftBar style='position:absolute;left:30px;top:83px;'></leftBar>
                    <leftpie style='position:absolute;left:30px;top:412px;'></leftpie>
                    <!-- 中 --> 
                    <maps  style='position:absolute;left:454px;top:128px;'></maps>
                    <!-- 右2 -->
                    <leftBar style='position:absolute;right:30px;top:83px;'></leftBar>
                    <rightBar style='position:absolute;right:30px;top:386px;'></rightBar>
                    <!-- 下2 -->
                    <bottomLine style='position:absolute;left:30px;top:770px;'></bottomLine>
                    <leftBar style='position:absolute;left:1077px;top:755px;'></leftBar> 
                </div>
            </div>
        </div>  
    </div>   
    </div>
</template>
<script > 
import maps from "./echarts/map/map.vue";
import leftBar from "./echarts/bar/left_bar/index.vue";
import headers from "./header/index.vue"; 
import rightBar from "./echarts/bar/right_bar/index.vue"; 
import leftpie from "./echarts/pie/left_pie/index.vue";
import bottomLine from './echarts/line/bottom_line/index.vue';
import store from "@/components/adminDataEcharts/store";
export default {
  name:"bigScreen",
  components:{
    maps,
    leftBar,
    headers, 
    rightBar, 
    leftpie, 
    bottomLine
  },
  data(){
    return {
      bg_src:require('@/assets/big_screen/bg.png'),
      scaleX:1,
      scaleY:1, 
      Height:store.state.height,
      Width:store.state.width
    }
  },
  computed:{ 
  },
  methods:{
    setHeight(){ 
      this.Height = window.innerHeight;
      store.setHeight(window.innerHeight);
    },
    setWidth(){ 
      this.Width = window.innerWidth;
      store.setWidth(window.innerWidth); 
    },
    init(){
      this.setHeight(); 
      this.setWidth();
      this.scaleX = this.Width / 1920; 
      this.scaleY = this.Height / 1080; 
    },
    changeScale(){   
      this.setHeight(); 
      this.setWidth();  
      let diff_width = this.Width / 1920  ;
      let diff_height =   this.Height  /1080;   
      this.scaleX =diff_width; 
      this.scaleY = diff_height;
    }, 
  },
  created(){
    this.init();
    let throttle = (fn, interval) => {
      let timer = null;
        return () => {
            if (timer) {
                return;
            }
            timer = setTimeout(() => {
                clearTimeout(timer);
                timer = null;
                typeof fn === 'function' ? fn() : "";
            }, interval)
        };
    }
    var _onresize = window.onresize || function(){};
    window.onresize = ()=>{      
      throttle(this.changeScale,500)();
      throttle( _onresize(),500)();  
    }
  }
} 
</script>
<style>
.wrapper_img{
  width:100%;
  height:100%; 
  background-position: center;
  background-size:100% 100%; 
  position: absolute;
  left: 0;
  top: 0; 
  bottom: 0;
  right:0; 
}
.content{
  overflow: hidden;
}
.content_inter{
  transition:0.5s linear all;
  width:1920px;
  height:1080px; 
  overflow: hidden;
} 
.bar_bg{ 
  background-image:url(~@/assets/big_screen/boderRadius_square.png);
  background-repeat: no-repeat;
  background-size:100% 100%;
}
.line_bg{
  background-image:url(~@/assets/big_screen/longBorderRadius_square.png);
  background-repeat: no-repeat;
  background-size:100% 100%;
}
</style>