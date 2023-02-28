<template>
  <div class="hello">
    <h1 >北京地铁预约服务</h1>
    <div id="applogin">
      <span>手机号:</span><br/>
      <input type="text" v-model="phone" placeholder="请输入手机号"><br/>
      <span>验证码：</span><br/>
      <input type="text" v-model="viCode"  placeholder="请输入验证码"/><br>
      <button type="submit" :disabled="disabled" @click="getVICode">{{ btntxt }}</button>
      <button type="submit" @click="applogin"> 登录</button>
    </div>
    <div class="tips">
      <p>提示：目前开放 
        <a href="javascript"  style="color: red;text-decoration:none;">沙河站,</a>
        <a href="javascript"  style="color: red;text-decoration:none;">天通苑站,</a>
        <a href="javascript"  style="color: red;text-decoration:none;">草房站</a>
        地铁预约进展测试！！！</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'HelloWorld',
  data () {
    return {
      phone:'',
      viCode:'',
      disabled: false,
      time: 60,
      btntxt: "发送验证码",
    }
  },
  mounted:function () {
    this.getClicks();
  },
  methods:{
    //获取手机号接口
    getVICode () {
      const header = {
        headers:{token: 'haoyujiayou'}
      }
      const phone = this.phone
      if (!phone) {
        alert('请输入手机号！')
        return false;
      }

      axios.get(`https://www.huyoa.com/api/getVlCode/?phone=${phone}`,header).then(res=>{
        console.log(res)
        if(res.data.status === 200) {
          alert('已发送验证码，请等待！')
        }else if(res.data.status !== 200){
          alert(res.data.body)
        }
      });
      //验证码倒计时
      this.time = 60;
      this.timer();
    },
    //发送手机验证码倒计时
    timer() {
      if (this.time > 0) {
        //console('test')
        this.disabled = true;
        this.time--;
        this.btntxt = this.time + "秒";
        setTimeout(this.timer, 1000);
      } else {
        this.time = 0;
        this.btntxt = "发送验证码";
        this.disabled = false;
      }
    },
     //登录接口
    applogin () {
      if (!this.phone) {
        alert('请输入手机号！')
        return false;
      }
      if (!this.viCode) {
        alert('请输入验证码！')
        return false;
      }
      const header = {
        headers:{token: 'haoyujiayou'}
      }
      const data = {
        phone: this.phone,
        verifyCode: this.viCode
      }
      axios.post(`https://www.huyoa.com/api/metorLogin`,data,header).then(res=>{
        console.log(res)
        if(res.data.status === 200) {
          alert('刷新权限成功！')
        }else if(res.data.status !== 200){
          alert(res.data.body)
        }
       })
    },
    //获取点击次数接口
    getClicks () {
      const header = {
        headers:{token: 'haoyujiayou'}
      }
      axios.get(`https://www.huyoa.com/api/getClicks`,header).then(res=>{
        console.log(res)
        if(res.data.status !== 200){
          alert(res.data.body)
        }
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hello {
  width: 375px;
  height:375px;
  border-radius: 50%;
  margin:0 auto ;
  margin-top:13%;
  text-align:center;
  background-color:rgba(244, 251, 251, 0.5);
}
.hello h1{
  text-align: center;
  color: aliceblue;
  padding-top: 20px;
}


#applogin span{
  text-align: center;
  font-size: 20px;
}
#applogin input{
  width:170px;
  font-size:18px;
  border-bottom:2px solid #a2a2a2;
  padding:5px 10px;
  color:#000000;
}
#applogin >button {
  margin-top:10px;
  width:190px;
  height:25px;
  font-size:18px;
  font-weight:700;
  color:#fff;
  background-image: linear-gradient(to right, #74ebd5 0%, #9face6 100%);
  border:0;
  border-radius:15px;
}
.tips {
  margin-top: 5px;
}
</style>
