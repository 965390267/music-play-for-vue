
    <template>
        <input type="button" value="静音" @click='novoice'><input type="button" value="播放" @click='playing'><br>
        <input type="button" value="上一曲" @click='last'><input type="button" value="下一曲" @click='next'><br>
        <label>音量：</label><input type='range'min="0.1" max="1" step="0.1" @change='vol()' ref='volume'><br>
        <label>播放进度：</label><input type='range'min="1" :max="maxlength" step="1" @change='range()' ref='progress'><br>
            <audio :src='musicsrc' @canplaythrough='loaded()' @timeupdate="time()" @ended="end()" ref='music'>您的浏览器版本过低，请升级您的浏览器</audio> 
            <p>{{songname}}</p>
<p>当前进度:{{currenttime}}</p>
<p>总时长:{{alltime}}</p>
    </template>

  
   <script>


     
export default {

  data(){
   return{
    currenttime:'',//已播放时间
    alltime:'',//歌曲总时长
    isplay:true,//是否播放状态
    isnovoice:false,//是否静音状态
    sing:['./bgm1.mp3','./bgm2.mp3','./bgm3.mp3','./bgm4.mp3','./bgm5.mp3','./bgm6.mp3','./bgm7.mp3','./bgm8.mp3','./bgm9.mp3','./bgm10.mp3'],//歌曲路径数组
    musicsrc:'',//双向绑定音乐路径
    index:0,//当前歌曲的索引,
    maxlength:'',
    songname:''
   }
  },
  methods:{
      
    loaded(){//歌曲加载完成可以播放后执行该函数
        this.$refs.music.play();
        this.alltime=this.$refs.music.duration
        this.$refs.music.volume=0.4;
        this.$refs.volume.value=0.4;
        // this.$refs.music.currentTime=0;
        // this.$refs.progress.value=0;
        this.songname=this.sing[this.index]
        let allmm,allss;
        this.maxlength=this.alltime;
         if(this.alltime<60){//把毫秒数转换为分和杪
            this.alltime= Math.round(this.alltime) ;
            if(this.alltime<10){//小于十秒前面加0
                this.alltime='0'+ Math.round(this.alltime) ;
            }

   }else{
     allmm=Math.floor(this.alltime/60);
   allss=Math.round(this.alltime-allmm*60) ;
 this.alltime=allmm+':'+allss;

   }
  },
  playing(){//播放以及暂停功能实现
      if(this.isplay){
        this.$refs.music.pause()
           this.isplay=!this.isplay;
           }else{
            this.$refs.music.play()
            this.isplay=!this.isplay; 
           }
     

  },
  novoice(){//静音功能实现
      this.isnovoice=!this.isnovoice;
this.$refs.music.muted= this.isnovoice;
   
  },
  time(){//当前歌曲已经播放的时间
      this.currenttime=this.$refs.music.currentTime;
let allmm,allss;
         if(this.currenttime<60){//把毫秒数转换为分和杪
            this.currenttime= Math.round(this.currenttime) ;
            if(this.currenttime<10){
                this.currenttime='0'+ Math.round(this.currenttime) ;
            }
   }else{
     allmm=Math.floor(this.currenttime/60);
   allss=Math.round(this.currenttime-allmm*60) ;
 this.currenttime=allmm+':'+allss;

   }
    // console.log(this.$refs.music.currentTime)
  },
  end(){//当前歌曲是否播放完成处理
    console.log(this.$refs.music.ended) ;
    if(this.$refs.music.ended){
        if(this.index<this.sing.length-1){
            this.index++;
        }else{
            this.index=0;
        }
        this.musicsrc=this.sing[this.index]
    }
  },
  next(){//下一曲
    if(this.index<this.sing.length-1){
            this.index++;
        }else{
            this.index=0;
        }
        console.log(this.index)
        this.musicsrc=this.sing[this.index]
  },
  last(){//上一曲
    if(this.index>0){
            this.index--;
        }else{
            this.index=this.sing.length-1;
        }
        console.log(this.index)
        this.musicsrc=this.sing[this.index]
  },
  vol(){//音量君
    console.log( this.$refs.volume.value) 
this.$refs.music.volume=this.$refs.volume.value
  },
  range(){//进度君
    console.log( this.$refs.progress.value);
  this.$refs.music.currentTime =this.$refs.progress.value;
  this.currenttime=  this.$refs.music.currentTime;

  }
  },
  mounted(){
   this.musicsrc=this.sing[this.index]
  }
}
      

   </script>
