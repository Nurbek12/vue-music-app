<template>
    <div class="eq">
        <!-- <div class="db" v-for="i in 30" :key="i"
        :style="`--i:${i};`">

        </div> -->
    </div>
</template>

<script>
export default {
    name:'Eq',
    props:['s'],
    mounted(){
        window.onclick = ()=>{
            const numb_of_bars = 30;
            const ctx = new AudioContext();
            const audioSource = ctx.createMediaElementSource(this.s);
            const analayser = ctx.createAnalyser();
            audioSource.connect(analayser);
            audioSource.connect(ctx.destination);
            const frequencyData = new Uint8Array(analayser.frequencyBinCount);
            analayser.getByteFrequencyData(frequencyData);
            const visualserContainer = document.querySelector('.eq');
            for(let i=0;i<numb_of_bars;i++){
                const bar = document.createElement('div');
                bar.setAttribute('id',"bar"+i);
                bar.setAttribute('style',"--i:"+i);
                bar.setAttribute('class','db');
                visualserContainer.appendChild(bar);
            }
            function renderFrame(){
                analayser.getByteFrequencyData(frequencyData);
                for(let i=0;i<numb_of_bars;i++){
                    const index = (i+15)*1;
                    const fd = frequencyData[index];
                    const bar = document.querySelector('#bar'+i);
                    if(!bar){
                        continue;
                    }
                    const barHeight = Math.max(4, fd/3 || 0);
                    bar.style.height = barHeight + 'px';
                }
                window.requestAnimationFrame(renderFrame);
            }
            renderFrame();
        }
    }
}
</script>

<style>
.eq{
    position: absolute;
    bottom: 0px;
    right: 0;
    left: 0;
    text-align: center;
}
.db{
    display: inline-block;
    margin: 0 1px;
    width: 7px;
    height: calc(1px*var(--i));
    background: linear-gradient(#32d8e4,#00fc22);
    animation: a 2s linear infinite;
    animation-delay: calc(.02s*var(--i));
}
@keyframes a{
    0%{
        filter: hue-rotate(0deg) saturate(2);
    }
    100%{
        filter: hue-rotate(20deg) saturate(2);
    }
}
</style>