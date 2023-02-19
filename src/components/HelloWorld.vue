<template>
  <div class="hello">
    <div id="app">
      <span>手机号：</span>
      <input type="text" v-model="phone" style="margin-right: 10px;">
      <button type="submit" @click="getVICode">获取验证码</button>
    </div>
    <div id="applogin">
      <span>验证码：</span>
      <input type="text" v-model="viCode" style="margin-right: 10px;">
      <button type="submit" @click= "applogin">登录</button>
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
      viCode:''
    }
  },
  props: {
    msg: String
  },
  methods:{
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
        if(res.data.status !== 200){
          alert(res.data.body)
        }
      })
    },
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
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.hello {
  margin: 0 auto;
  width: 350px;
}
#app {
  margin-bottom: 20px;
  text-align: left;
}
#applogin {
  text-align: left;
}
</style>
