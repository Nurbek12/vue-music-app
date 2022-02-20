<template>
  <section>
    <div :class="`phone ${transf?'on':''}`">
      <div id="artistImg">
        <video
          autoplay
          ref="vid"
          :src="newMus.video"
          muted
          loop
        ></video>
        <Eq :s="song"/>
        <h2 id="musicTitle">{{newMus.title}}</h2>
      </div>
      <div id="musicAlbums">
        <p>Albums</p>
        <Swip :musicL="musicList" @isPl="isplay($event)"/>
        <!-- <swiper
          :slides-per-view="3"
          :space-between="50"
          @swiper="onSwiper"
          @slideChange="onSlideChange"
        >
          <swiper-slide v-for="(music, i) in musicList" :key="i">
            <img :src="music.img" />
              <h3 class="content">
                {{music.title}}
                <br /><span>{{ '2021' }}</span>
              </h3>
          </swiper-slide>
        </swiper> -->
      </div>
      <ul id="musicList">
        <p><span>Host popular</span> All</p>
        <li id="music" v-for="(music, i) in musicList" :key="i" 
        @click="isplay(i)">
          <span>{{music.title}}</span>
          <div class="icon">
            <div class="fa fa-heart" :style="{color:`${music.loveMus?'#f20':'#aaa'}`}"></div>
            <div class="fa fa-ellipsis-h"></div>
          </div>
        </li>
        <audio :src="newMus.music" ></audio>
      </ul>
      <Cont 
        :tr="transf" 
        :pl="isplaying" 
        :mus="newMus"
        :rp="replay"
        :rnds="randomSong"
        :tm="tw"
        :tim="timeExp"
        :d="dur"
        @tiemLeft="timeLe($event)"
        @lovMus="LoveMusc()"
        @transfr="transf=!transf"
        @playpass="playM()"
        @prevM="prev"
        @nextM="next"
        @replMus="replaySong"
        @randMus="randSong"
        />
    </div>
  </section>
</template>

<script>
import Cont from "./components/Contro.vue";
import musics from "./db";
// import { Swiper, SwiperSlide } from 'swiper/vue';
// import 'swiper/swiper.scss';
import Swip from './components/Swipe.vue'
import Eq from './components/Eq'

export default {
  name: "App",
  data() {
    return {
      musicList: musics,
      index: 0,
      isplaying: false,
      transf: true,
      newMus: [],
      replay: false,
      randomSong: false,
      song: new Audio(),
      tw:0,
      dur: '',
      timeExp:'',
      video: this.$refs.vid,
    };
  },
  methods: {
    // onSwiper(swiper) {
    //   console.log(swiper);
    // },
    // onSlideChange() {
    //   console.log('slide change');
    // },
    playM() {
      this.isplaying=!this.isplaying;
      if(this.isplaying){
        this.song.play();
      }else{
        this.song.pause();
      }
    },
    isplay(i){
      this.newMus = this.musicList[i];
      this.song.src = this.newMus.music;
      this.isplaying = true;
      this.song.play();
      this.index = i;
    },
    next() {
      if(this.randomSong){
        const randnum = Math.floor(Math.random()*this.musicList.length);
        this.index = randnum;
      }else{
        this.index++;
        if(this.index > this.musicList.length - 1){
          this.index = 0 ;
        }
      }
      this.isplay(this.index);
    },
    prev() {
      this.index--;
      if(this.index < 0){
        this.index = this.musicList.length - 1;
      }
      this.isplay(this.index);
    },
    LoveMusc(){
      this.musicList[this.index].loveMus = !this.musicList[this.index].loveMus;
    },
    replaySong(){
      this.replay=!this.replay;
    },
    randSong(){
      this.randomSong=!this.randomSong;
    },
    timeLe(e){
      const d = this.song.duration;
      this.song.currentTime = (e[1] / e[0] )*d;
    },
  },
  created(){
    this.newMus = this.musicList[0];
    this.song.src = this.newMus.music;
    this.song.volume = 0.5;
  },
  components: {
    Cont,
    Swip,
    Eq,
    // Swiper,
    // SwiperSlide,
  },
  mounted(){
    window.addEventListener('keypress', e=>{
      if(e.keyCode === 32){
        this.playM();
      }
    })
    this.song.addEventListener('timeupdate',(e)=>{
      const { duration ,currentTime } = e.srcElement;
      this.tw = (currentTime/duration)*100;
      let m,a = 0;
      this.dur = duration;
      
      a = parseInt(currentTime);
      m = Math.floor(a/60);
      if(m != 0 && a> 60*m - 1){
        a = a % (m * 60);
      }
      this.timeExp = `${m}:${a<10?'0'+a:a}`;
      if(this.tw==100){
        if(this.replay){
          this.isplay(this.index);
        }else{
          this.next();
        }
      }
    });
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: sans-serif;
}
section {
  position: absolute;
  width: 100%;
  height: 100vh;
  background: linear-gradient(-45deg, #0d0ba3, #008cff);
  display: flex;
  justify-content: center;
  align-items: center;
}
.phone ul#musicList p,
.phone ul#musicList li#music,
.phone #musicplayer,
.phone #musicplayer #runner p,
.phone #musicplayer .btns,
.phone #musicplayer .icons {
  display: flex;
  align-items: center;
}
.phone {
  position: relative;
  width: 290px;
  height: 590px;
  border-radius: 40px;
  box-shadow: 0px 25px 20px #0003;
  overflow: hidden;
  background: #131323;
  border: 7px solid #222;
}
.phone::after{
  position: absolute;
  content: '';
  width: 160px;
  height: 20px;
  border-radius:0px 0px 10px 10px;
  background: #222;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
}
.phone::before{
  position: absolute;
  content: '';
  width: 130px;
  height: 7px;
  border-radius:10px;
  background: #444;
  top: 6px;
  left: 50%;
  z-index: 2;
  transform: translateX(-50%);
}
.phone #artistImg {
  position: relative;
  width: 100%;
  left: 0;
  transition: 0.3s;
  top: 0;
  height: 200px;
  box-shadow: inset 0px -60px 50px #131323;
}
video {
  position: absolute;
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.phone #artistImg::after {
  position: absolute;
  width: 100%;
  height: 100%;
  z-index: 1;
  content: "";
  background: linear-gradient(0deg, #13132388, #fff0);
}
.phone #artistImg #musicTitle {
  position: absolute;
  color: #fff;
  left: 15px;
  z-index: 2;
  font-size: 1.4em;
  bottom: 20px;
}
.phone #musicAlbums {
  position: relative;
  width: 100%;
  height: 160px;
}
.phone #musicAlbums p {
  position: absolute;
  top: 10px;
  left: 15px;
  color: #fff;
  font-size: 0.89em;
  font-weight: 600;
}
.phone ul#musicList {
  position: relative;
  width: 100%;
  height: 140px;
  bottom: 0;
  overflow-y: auto;
}
.phone ul#musicList::-webkit-scrollbar {
  display: none;
}
.phone ul#musicList p {
  position: relative;
  width: 100%;
  top: 0;
  justify-content: space-between;
  padding: 15px;
  color: #fff;
}
.phone ul#musicList li#music {
  position: relative;
  width: 100%;
  cursor: pointer;
  color: #fffb;
  justify-content: space-between;
  padding: 10px 15px 10px 15px;
  font-size: 0.8em;
}
.phone ul#musicList li#music .icon .fa {
  margin-right: 10px;
}
</style>
