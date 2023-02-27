<template>
  <div class="hello">
    <div id="app">
      <span>手机号：</span><br/>
      <input  
        type="text" 
        v-model="phone" 
        style="margin-right: 10px;"
        placeholder="请输入手机号">
    </div>
    <div id="applogin">
      <span>验证码：</span><br/>
      <input type="text" v-model="viCode"  placeholder="请输入验证码"/>
      <button type="submit" :disabled="disabled" @click="getVICode">{{ btntxt }}</button>
    </div>
    <div id="register">
      <button type="submit" 
              class="denglu"
              @click="applogin">
              登录
      </button>
    </div>
    <div class="tips">
      <p>提示：目前开放 沙河站、天通苑站、草房站地铁预约进展测试!!!</p>
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
  margin: 0 auto;
  width: 350px;
  border: 1px solid #000000;
  border-radius: 10px;
  background-color: rgb(224, 222, 222);
}
#app {
  margin: 120px 0 20px 30px;
  text-align: left;
}
#applogin {
  text-align: left;
  margin-bottom: 20px;
  margin-left: 30px;
}
.denglu {
  margin-left: -40px;
  width: 250px;
  background-color: rgb(224, 222, 222);
}
.tips {
  margin-bottom: 100px;
}
</style>
