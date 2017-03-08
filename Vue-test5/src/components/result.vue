<template>
  <div id="result" flex="main:center dir:top cross:center">
    <img class='title-hide' :src="titleurl"  alt="">
    <div class='answerlast' flex="main:center dir:top cross:center">
      <p v-if="answershow" class='answer-tit'>恭喜你在本次测试中考了<span>{{score}}</span>分,打败了全国<span>{{percent}}</span>的玩家！</p>
      <p v-if="others" class='answer-tit'>您的好友在本次测试中获得<span>{{score}}</span>分，祝您超过您的好友！</p>
      <p class='answer-mes'>{{conclusion}}</p>
      <div class='btn-cli' flex="main:center cross:center">
        <button class='again' @click="again"></button>
        <button class='share' @click="share" v-if="shareshow"></button>
      </div>
    </div>
    <div class='wrap-b' v-if="wrapbshow" flex="main:center cross:center">
      <img :src="ewmurl" alt="">
    </div>
    <div v-if="maskshow" class='mask' flex="dir:top cross:center" @click="mask">
        <div class='mask-t'></div>
        <img class='mask-b' v-if='shareurlshow' :src="shareurl" alt="">
    </div>
  </div>
</template>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#result{
    width: 100%;
    height: 100%;
}
.title-hide{
    height:0;
}
.answerlast{
    width: 83.7%;
    height:66%;
    background: url(../assets/img/Answer2.png) no-repeat center;
    background-size: contain;
    position: relative;
}


.btn-cli{
    position: absolute;
    bottom: 0rem;
    width: 100%;
}

.btn-cli button{
    border: 0;
    width: 2.2rem;
    height:0.86rem;
}

.btn-cli .again{
    background: url(../assets/img/icon/btn3.png) no-repeat center;
    background-size: cover;
    margin-right: 0.14rem;
}

.btn-cli .share{
    background: url(../assets/img/icon/btn4.png) no-repeat center;
    background-size: cover;
}

.answerlast p{
    color: #4b2508;
    font-size: 0.34rem;
    font-weight: 900;
    max-width: 4.2rem;
    line-height: 0.4rem;
}


.answer-tit{
    margin-top: 1rem;
}

.answer-tit span{
    color: #ff9205;
    font-size: 0.4rem;
    font-weight: 900;
}

.answerlast .answer-mes{
    color: green;
    text-indent:2em;
}

.wrap-b{
    width:100%;
    height: 34%;
}

.wrap-b img{
    width:100%;
}



.mask{
    position: absolute;
    left: 0;
    top:0;
    width: 100%;
    height: 100%;
}
.mask .mask-t{
    position: absolute;
    width: 100%;
    height: 100%;
    background: #000;
    opacity: 0.6;
    -webkit-opacity: 0.6;  
    /* Netscape and Older than Firefox 0.9 */  
    -moz-opacity: 0.6;  
    /* Safari 1.x (pre WebKit!) 老式khtml内核的Safari浏览器*/  
    -khtml-opacity: 0.6;  
    /* IE9 + etc...modern browsers */  
    opacity: .6;  
    /* IE 4-9 */  
    filter:alpha(opacity=60);  
    /*This works in IE 8 & 9 too*/  
    -ms-filter:"progid:DXImageTransform.Microsoft.Alpha(Opacity=60)";  
    /*IE4-IE9*/  
    filter:progid:DXImageTransform.Microsoft.Alpha(Opacity=60);  
    z-index: 100;
}
.mask .mask-b{
    z-index: 200;
    width: 5.33rem;
    height: 2.4rem;
    margin-top: 10%;
}



</style>

<script>
import 'assets/js/zepto-v1.2.0.js';

export default {
  name: 'result',
  data () {
    return {
      score : 0,
      percentage : '',
      result2 :["阴阳师", "坦克世界", "王者荣耀", "仙剑奇侠传", "魔兽世界", "冒险岛", "保卫萝卜", "梦幻西游", "拳皇", "征途"],
      countAll:'',
      ewmurl: require('../assets/img/ewm.png'),
      shareurl : require('../assets/img/icon/sharee.png'),
      countOne:'',
      conclusion:'',
      percent:'',
      maskshow: false,
      titleurl : require('assets/img/tit.jpg'),
      shareshow : true,
      answershow : true,
      shareurlshow: true,
      others: false,
      wrapbshow: true
    }
  },
  created :function(){
      console.log('created')
      change()
      //动态改变title
        function change(username1){
            var $body = $('body');
            document.title = '脑洞猜游戏，能及格的都不是一般银~';
            document.setTitle = function(t) {
            document.title = t;
            var i = document.createElement('iframe');
            i.src = '//m.baidu.com/favicon.ico';
            i.style.display = 'none';
            i.onload = function() {
                setTimeout(function(){
                    i.remove();
                }, 9)
                }
                document.body.appendChild(i);
            }
        }



    if(!localStorage.hasOwnProperty('guess')){
            this.shareurlshow = false;
            this.shareshow = false;
            this.answershow = false;
            this.others = true;
            this.wrapbshow = false
    }



    //获取url参数传给this.score
    if(this.$store.state.count.length == 0){
        this.$store.state.count = ['','','','','','','','','','',]
    }



    this.score = parseInt(this.$route.query.score);


    
    //查询数据库返回结果
      this.$http.get('/api/login/getAll')
      .then((response) => {
        //console.log(response.data)
        this.countAll = response.data;
        // 响应成功回调
        return this.$http.post('/api/login/getOne/',{name : this.score});
      })
      .then((response) => {
         //console.log(response.data)
         this.countOne = response.data;

         let params = { 
          account : this.score,
          password : this.$store.state.count
        };
        // 创建一个账号
        return this.$http.post('/api/login/createAccount',params).then(response => {
            // success callback
            //console.log(response)
            //百分比
            this.percent = toDecimal(this.countOne/this.countAll) + '%'
            function toDecimal(x) { 
                var f = parseFloat(x); 
                if (isNaN(f)) { 
                    return; 
                } 
                f = Math.round(x*10000)/100; 
                return f; 
            } 


        }, response => {
            // error callback
            console.log(error)
        })
      })
      .catch((reject) => {
        console.log(reject)
      });

      switch (this.score){
          case 0:
              this.conclusion = '脑洞这东西，你连是什么都不知道吧，没准还真以为是脑袋上的洞呢。';
              break;
          case 10:
              this.conclusion = '最实际的你脑洞简直是精密计算，脑洞是啥？算错亏了怎么办，赔了可不行。';
              break;
          case 20:
              this.conclusion = '理性的行动哲学，被自己的脑洞塞成正方形的，万一研究出奇奇怪怪的东西，毁灭地球怎么办？';
              break;
          case 30:
              this.conclusion = '任何稍显浮夸不切实际的东西，你都是摒弃的，别说是脑洞了，就连一些有些过头的联想，你都不想多鸟一眼。';
              break;
          case 40:
              this.conclusion = '单纯的你，神经大条而且很直，不懂得转弯，经常会听不懂别人的内涵段子以及话外之音。';
              break;
          case 50:
              this.conclusion = '你推理的时候脑洞就大到不行，发现一条暧昧短信就已经想好怎么手撕仇人的场景了。';
              break;
          case 60:
              this.conclusion = '脑洞大的你行动力还强，结果撞得头破血流还是要把墙撞碎，你就是要从墙中间过去。';
              break;
          case 70:
              this.conclusion = '有句话叫做“你猜到了故事的开头，却没有猜到故事的结尾”，你的脑洞给人的第一印象。';
              break;
          case 80:
              this.conclusion = '脑洞太随性，想干嘛干嘛，像个细胞一样，1个变2个，2个变4个……分分钟发展出一个家族。';
              break;
          case 90:
              this.conclusion = '脑洞不大的话，怎么幻想你的偶像伦理梦幻爱情剧，不能幻想活着又有什么意思？';
              break;
          case 100:
              this.conclusion = '你在母星被研究了几百年，因无法破解你的脑洞和冲动的联系，所以被扔到了地球上。';
              break;
          default:
              console.log("程序错误");
              break;
      }

  },
  //挂载结束状态
  mounted: function(){
    console.log('mounted')


    if(!localStorage.hasOwnProperty('guess')){
          this.conclusion = this.conclusion.replace(/你/g, '他/她'); 
    }else{
        localStorage.removeItem("guess");
    }
     

  },
  methods: {
      again: function(){
          this.$router.push({ path: '/' });
      },
      share: function(){
        this.maskshow = true;

      },
      mask: function(){
        this.maskshow = false;

      }
        
  }
}
</script>


