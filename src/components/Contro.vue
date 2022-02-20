<template>
  <div id="musicplayer">
    <p id="isToggle1">
      New playing<br />
      <span>My playlist</span>
    </p>
    <div id="imgMusic" @click="$emit('transfr')"
    :style="`--img:url(${mus.img})`">
    </div>
    <div class="titl">
      <h2 id="Artist">{{ mus.artitst }}</h2>
      <h3 id="Title">{{ mus.title }}</h3>
    </div>
    <div id="runner" ref="run">
      <div id="circ" :style="{width:`${tm}%`}"></div>
      <p>
        <span id="time">{{ tim || '0:00' }}</span>
        <span id="dur">{{`${(Math.floor(d/60)||'0')}:${(Math.floor(d%60)||'0')>10?(Math.floor(d%60)||'0'):'0'+(Math.floor(d%60)||'0')}`|| '0:00'}}</span>
      </p>
    </div>
    <div class="btns">
      <div id="back" class="btn" @click="$emit('prevM')">
        <i class="fa fa-backward"></i>
      </div>
      <div id="play" @click="$emit('playpass')">
        <i :class="`fa fa-${pl?'pause':'play'}`"></i>
      </div>
      <div id="forw" class="btn" @click="$emit('nextM')">
        <i class="fa fa-forward"></i>
      </div>
    </div>
    <div class="icons">
      <i class="fa fa-refresh" id="repl" @click="$emit('replMus')"
      :style="{color:`${rp?'#f20':'#aaa'}`}"></i>
      <i class="fa fa-heart" id="hrt" @click="$emit('lovMus')"
      :style="{color:`${mus.loveMus?'#f20':'#aaa'}`}"></i>
      <i class="fa fa-random" id="rand" @click="$emit('randMus')"
      :style="{color:`${rnds?'#f20':'#aaa'}`}"></i>
    </div>
  </div>
</template>

<script>
export default {
  name: "Cont",
  props:['tr','pl','mus','rp','rnds','tm','tim','d'],
  mounted(){
    this.$refs.run.addEventListener('click',e=>{
        const w = this.$refs.run.clientWidth;
        const c = e.offsetX;
        this.$emit('tiemLeft',[w,c]);
    });
  }
};
</script>

<style>
.phone #musicplayer {
  position: absolute;
  width: 100%;
  border-radius: 40px;
  background: #fff;
  height: 92%;
  z-index: 2;
  bottom: -6px;
  transition: all 0.5s;
  flex-direction: column;
}
.phone #musicplayer p {
  position: relative;
  margin-top: 25px;
  font-size: 0.95em;
  font-weight: bold;
  text-align: center;
  cursor: pointer;
  line-height: 20px;
}
.phone #musicplayer p span {
  color: #888;
  font-size: 0.87em;
  font-weight: normal;
}
.phone #musicplayer #imgMusic {
  position: relative;
  width: 200px;
  height: 200px;
  border-radius: 40px;
  filter: drop-shadow(2px 7px 15px #0005);
  background: var(--img);
  background-size: cover;
  background-position: center;
  margin-top: 20px;
  transition: all 0.3s;
  cursor: pointer;
}
.phone #musicplayer .titl {
  text-align: center;
}
.phone #musicplayer .titl h2#Artist {
  font-size: .9em;
  /* text-align: center; */
  max-width: 200px;
  white-space: normal;
  margin: 20px 0 2px 0;
  font-weight: 700;
}
.phone #musicplayer .titl h3#Title {
  color: #999;
  font-size: .76em;
  margin-top: 5px;
}
.phone #musicplayer #runner {
  position: relative;
  width: 80%;
  cursor: pointer;
  height: 4px;
  background: #ddd;
  border-radius: 4px;
  margin-top: 35px;
}
/* .phone.on #musicplayer .titl h2#Artist {
  font-size: .8em;
}
.phone.on #musicplayer .titl h3#Title {
  font-size: .8em;
} */
.phone #musicplayer #runner #circ {
  position: absolute;
  width: 0%;
  top: 0;
  transition: 0.2s;
  left: 0;
  height: 100%;
  background: #000;
  border-radius: 4px;
}
.phone #musicplayer #runner #circ::after {
  position: absolute;
  width: 10px;
  height: 10px;
  border-radius: 50%;
  border: 2px solid #fff;
  background: #000;
  content: "";
  right: -3px;
  top: -5px;
  box-shadow: 0px 0px 5px #0009;
}
.phone #musicplayer #runner p {
  position: absolute;
  width: 100%;
  font-size: 0.8em;
  justify-content: space-between;
  top: -15px;
}
.phone #musicplayer .btns {
  position: relative;
  width: 100%;
  padding: 10px;
  margin-top: 20px;
  justify-content: space-evenly;
}
.phone #musicplayer .btns .btn {
  position: relative;
  width: 40px;
  height: 40px;
  border: none;
  border-radius: 50%;
  cursor: pointer;
  text-align: center;
  line-height: 40px;
}
.phone #musicplayer .btns #play {
  position: relative;
  width: 55px;
  background: #0001;
  height: 55px;
  border: none;
  font-size: 1.1em;
  border-radius: 50%;
  cursor: pointer;
}
.phone #musicplayer .btns #play .fa {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.phone #musicplayer .btns #play:active {
  background: #ccc;
}
.phone #musicplayer .icons {
  position: relative;
  width: 60%;
  margin-top: 10px;
  justify-content: space-between;
}
.phone #musicplayer .icons .fa {
  position: relative;
  color: #aaa;
  width: 13px;
  height: 13px;
  cursor: pointer;
}
.phone.on #musicplayer {
  height: 14%;
  flex-direction: row;
  justify-content: space-between;
  padding: 0px 15px; 
}
.phone.on #musicplayer p {
  display: none;
}
.phone.on #musicplayer #imgMusic {
  position: relative;
  width: 50px;
  height: 50px;
  /* left: 0px; */
  margin-top: 0;
  border-radius: 50%;
}
.phone.on #musicplayer .titl {
  position: absolute;
  text-align: left;
  left: 77px;
}
.phone.on #musicplayer .titl h2#Artist {
  font-size: .72em;
  margin: 2px 0 2px 0;
  font-weight: bold;
}
.phone.on #musicplayer .titl h3#Title {
  color: #999;
  font-size: 0.72em;
  text-align: left;
  margin-top: 3px;
}
.phone.on #musicplayer #runner {
  display: none;
}
.phone.on #musicplayer .btns {
  width: 30%;
  margin-top: 0;
  transform: translateX(10px);
}
.phone.on #musicplayer .btns .btn {
  display: none;
}
.phone.on #musicplayer .icons {
  display: none;
}
.volume {
  position: relative;
  width: 30px;
  height: 160px;
  border-radius: 20px;
  margin: 10px;
  border: 1px solid #000;
  background: #444;
  cursor: pointer;
  overflow: hidden;
}
.volume .vol {
  position: absolute;
  width: 100%;
  height: 0%;
  bottom: 0;
  transition: 0.3s;
  left: 0;
  background: #aaa;
}
</style>