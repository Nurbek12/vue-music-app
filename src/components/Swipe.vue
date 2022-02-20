<template>
  <div class="swiper-container">
    <div class="swiper-wrapper" ref="scroll">
        <div class="btn pr" @click="scrollTable(-141)"
        v-if="pr">
            <i class="fa fa-angle-left"></i>
        </div>
        <div
            class="swiper-slide"
            v-for="(music, i) in musicL"
            :key="i"
            @click="$emit('isPl',i)"
        >
            <img :src="music.img" />
            <h3 class="content">
            {{ music.title }}
            <br /><span>2021</span>
            </h3>
        </div>
        <div class="btn nx" @click="scrollTable(141)"
        v-if="nx">
            <i class="fa fa-angle-right"></i>
        </div>
    </div>
    <!-- Add Pagination 
            @mousedown="swiper($event)" 
            
           <div class="swiper-pagination"></div>  -->
  </div>
</template>

<script>
export default {
    name:'Swip',
    props:['musicL'],
    data(){
        return{
            nx: true,
            pr: false,
        }
    },
    methods:{
        swiper(e){
            const cx = e.clientX;
            const ox = e.offsetX;
            const wd = this.$refs.scroll.clientWidth;
            console.log(cx);
            console.log(ox);
            const vb = ox/wd;
            this.$refs.scroll.scrollLeft = vb;
        },
        scrollTable(e){
            const sc = this.$refs.scroll;
            if(sc.scrollLeft == 0){
                this.pr = false;
                this.nx = true;
            }else if(sc.scrollLeft == 564){
                this.pr = true;
                this.nx = false;
            }else if(0<sc.scrollLeft<564){
                this.pr = true;
                this.nx = true;
            }
            sc.scrollLeft += e;
        }
    },
};
</script>

<style>
.swiper-container {
    width: 93%;
    display: flex;
    padding-top: 30px;
    margin-left: 10px;
}
.swiper-container .swiper-wrapper{
    width: 100%;
    display: flex;
}
.swiper-slide {
    background-position: center;
    background-size: cover;
    width: 100px;
    height: 120px;
    padding: 0 5px;
    margin: 2px;
}
.swiper-slide img {
    position: relative;
    width: 90px;
    height: 90px;
    object-fit: cover;
    border-radius: 15px;
}
.swiper-slide .content {
    position: relative;
    font-size: 0.9em;
    color: #fff;
    font-weight: lighter;
    line-height: 15px;
}
.swiper-slide .content span {
    position: relative;
    font-size: 0.7em;
    color: #aaa;
}
.swiper-wrapper{
    overflow-x: scroll;
    overflow-y: hidden;
    scroll-behavior: smooth;
    scroll-snap-type: proximity;
    width: 100%;
    padding:0 1px;
}
.swiper-wrapper::-webkit-scrollbar{
    height: 0;
}
.swiper-slide{
    cursor: pointer;
}
.btn{
    position: absolute;
    width: 20px;
    height: 20px;
    border-radius: 50%;
    background-color: #fff;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 2;
    cursor: pointer;
}
.btn.pr{
    top: 50%;
    transform: translate(0px,-50%);
    left: 13px;
}
.btn.nx{
    top: 50%;
    transform: translate(0px,-50%);
    right: 13px;
}
</style>