<template>  

   <div class="questions">
     <div v-if="firstShow" class="firstShow"  flex="main:center dir:top cross:center">
        <img class='title' :src="titleurl" alt="">
        <img class='title2' :src="titleurl2" alt="">
        <button class='open' @click="open"></button>
      </div>  
      <div v-if="secondShow" flex="dir:top main:center cross:center" class="secondShow">
        <div class='ques' flex="main:center dir:top cross:center">
            <span>{{thequestion}}</span>
            <img :src="imgurl" alt="">
            <input type="text" class='submit' v-model="message"  @focus="lose" @blur="lose2">
            <button class='start' @click="nextone"></button>
        </div>
         
      </div>
   </div>  
  
</template>  


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.questions{
  height: 100%;
  
}



.firstShow{
  height: 100%;
}

.title{
  width: 85%;
  height: 1.59rem;
}

.title2{
  margin-top: 0.4rem;
  margin-bottom: 0.4rem;
  width: 90.4%;
  height: 44.8%;
}


.open{
  border: 0;
  width: 3.23rem;
  height: 1.25rem;
  background: url(../assets/img/icon/btn1.png) no-repeat center;
  background-size: cover;
}



/*secondShow*/
.secondShow{
  width: 100%;
  height: 100%;
  
}

.ques{
  width: 90.6%;
  height: 63.7%;
  background: url(../assets/img/icon/Answer.png) no-repeat top center;
  background-size: contain;
  position: relative;
  padding-bottom: 1.45rem;
}

.ques img{
  padding-top: 1rem;
  width: 76%;
}

.ques span{
  position: absolute;
  color: #647c60;
  font-weight: 900;
  font-size: 0.5rem;
  top: 8%;
  left: 50%;
  transform: translate(-50%, 0);
  -ms-transform:translate(-50%, 0);	/* IE 9 */
  -moz-transform:translate(-50%, 0);	/* Firefox */
  -webkit-transform:translate(-50%, 0);/* Safari 和 Chrome */
  -o-transform:translate(-50%, 0);
}

.submit{
  width: 3.18rem;
  height: 0.84rem;
  border: 0;
  color: #4a5d4a;
  font-weight: 900;
  text-indent: 0.2rem;
  line-height: 0.84rem;
  font-size: 0.44rem;
  background: url(../assets/img/icon/input1.png) no-repeat center;
  background-size: cover;
  position: absolute;
  bottom: 1.45rem;
  left: 50%;
  transform: translate(-50%, 0);
  -ms-transform:translate(-50%, 0);	/* IE 9 */
  -moz-transform:translate(-50%, 0);	/* Firefox */
  -webkit-transform:translate(-50%, 0);/* Safari 和 Chrome */
  -o-transform:translate(-50%, 0);
}


.start{
  position: absolute;
  left: 50%;
  transform: translate(-50%, 0);
  -ms-transform:translate(-50%, 0);	/* IE 9 */
  -moz-transform:translate(-50%, 0);	/* Firefox */
  -webkit-transform:translate(-50%, 0);/* Safari 和 Chrome */
  -o-transform:translate(-50%, 0);
  border: 0;
  bottom: 0;
  width: 3.23rem;
  height: 1.25rem;
  background: url(../assets/img/icon/btn2.png) no-repeat center;
  background-size: cover;
}

.transtop{
  transform: translate(0,-40%);
  -ms-transform:translate(0,-40%);	/* IE 9 */
  -moz-transform:translate(0,-40%);	/* Firefox */
  -webkit-transform:translate(0,-40%);/* Safari 和 Chrome */
  -o-transform:translate(0,-40%);
}


</style>


<script>
export default {
  name: 'start',
  data () {
    return {
      firstShow: true,
      secondShow: false,
      thequestion: 1,
      score:'',
      result2 :["阴阳师", "坦克世界", "王者荣耀", "仙剑奇侠传", "魔兽世界", "冒险岛", "保卫萝卜", "梦幻西游", "拳皇", "征途"],
      titleurl: require('../assets/img/icon/title.png'),
      titleurl2: require('../assets/img/blackboard.png'),
      imgurl: require('../assets/img/1.png'),
      inputurl : require('../assets/img/icon/input2.png'),
      inputurl2 : require('../assets/img/icon/input1.png'),
      message: '',
      result1 :[]
    }
  },
  created :function(){
      if(!window.localStorage){
            alert("很抱歉我们不能存储些东西");
            return false;
        }else{
            var storage=window.localStorage;
            storage.setItem("guess",'一起进击吧！');

        }

        


  },
  methods: {
        //操作函数
        lose: function(){
            console.log($(window).height())
            $('html').css({'width':$(window).width(),'height':$(window).height()}); 
            $('.ques').addClass('transtop')

            
            $(window).resize(function() {
                $('.ques').removeClass('transtop')
            });
              
            //$(".ques").scrollTop( 200 + "px")
            $('.submit').val('').css({'background-image':'url('+this.inputurl+')'});
        		

        },
        lose2: function(){
          
         // $('.ques').removeClass('transtop')
          setTimeout(function(){
            $('.ques').removeClass('transtop')
          },250)

            if($('.submit').val('')){
                $('.submit').val('').css({'background-image':'url('+this.inputurl2+')'});
            }
        },
        open: function () {
            this.firstShow = false;
            this.secondShow = true;
        },
        nextone: function () {
            //组件跳转
            //this.$router.push({ path: '/result' })
            
            console.log(this.thequestion)
            this.result1.push(this.message);
            console.log(this.result1)
            this.thequestion++;

            if(this.thequestion == 11){
                //改变全局变量
                this.$store.state.count = this.result1;
                //数组比对比对得到的分数
                var arr1 =  this.result2;  
                var arr2 = this.result1; 
                var temp = [];   
                var temparray = [];  
                for (var i = 0; i < arr2.length; i++) {
                    temp[arr2[i]] = true; 
                };
                for (var i = 0; i < arr1.length; i++) {
                    if (!temp[arr1[i]]) {
                        temparray.push(arr1[i]); 
                    };
                };
                var CorrectNumber= 10-temparray.length;
                this.score = CorrectNumber*10;

                this.$router.push({ path: '/result?score='+this.score+'' });
                return false;
            }
            this.imgurl = require('../assets/img/'+this.thequestion+'.png');
            
            $('.submit').css({'background-image':'url('+this.inputurl2+')'});
            this.message = '';



        }
    }
    
}
</script>


