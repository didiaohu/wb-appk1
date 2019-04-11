<template lang="html">
  <div class="login">
    <div class="login-header">
      <!-- <i @click="showLogin" class="icon-back"></i> -->注册
    </div>

    <div class="input">
      <input type="text" placeholder="请输入账号">
      <input type="text" placeholder="请输入新密码">
      <p class="input__p"><input v-model="isChecked" class="input__checkbox" type="checkbox"/><span @click="isShow = !isShow">注册协议</span><span class="input__span" v-show="isShowTips"> *请同意注册协议</span></p>
      <button @click.stop.prevent="check('e052dce6-9b8e-4c04-b38d-46f25166c050')">注册</button>
    </div>
    <div class="tips" @click="goRegister">
      <ul>
        <li>登录</li>
      </ul>
    </div>
    <div class="mask" v-show="isShow" @click="isShow = !isShow">
      <div class="mask__content">
        1、注册条款的接受 
        请您认真阅读本协议尤其是免除或者限制有缘网责任的条款及其它限制会员权利的条款，一旦会员注册即表示会员已经阅读并且同意与本社区达成协议，完全理解并接受所有的注册条款。
        <br/><br/>
        2、会员注册条件 
        1) 申请注册成为本社区的会员应同时满足下列全部条件： 
        在注册之日必须年满18周岁以上。 
        2) 为更好的享有本社区提供的服务，会员应：  
        向本社区最新及完整的资料； 随时更新登记资料； 提供真实有效的联系人手机号码。 
        <br/><br/>
        3、会员账号名称安全 
        任何注册和使用的有缘网账号名称，不得有下列情形： 
        （一）违反宪法或法律法规规定的； 
        （二）危害国家安全，泄露国家秘密，颠覆国家政权，破坏国家统一的； 
        （三）损害国家荣誉和利益的，损害公共利益的； 
        （四）煽动民族仇恨、民族歧视，破坏民族团结的； 
        （五）破坏国家宗教政策，宣扬邪教和封建迷信的； 
        （六）散布谣言，扰乱社会秩序，破坏社会稳定的； 
        （七）散布淫秽、色情、赌博、暴力、凶杀、恐怖或者教唆犯罪的； 
        （八）侮辱或者诽谤他人，侵害他人合法权益的； 
        （九）含有法律、行政法规禁止的其他内容的。 
        会员以虚假信息骗取账号名称注册，或其账号头像、简介等注册信息存在违法和不良信息的，有缘网有权采取通知限期改正、暂停使用、注销登记等措施。 
        对冒用关联机构或社会名人注册账号名称的会员，有缘网有权注销其账号。会员应采取适当措施保障注册用户名和密码的机密性并对以其用户名和密码进行的所有活动承担全部责任。会员应确保于每次会话结束后退出帐户，若发现未经授权使用其用户名或密码或其他侵犯其帐户安全的任何行为立即通知有缘网；有缘网对未遵守上述规定而给会员或他人带来的任何损失不承担责任。 
        <br/><br/>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'register',

  data() {
    return {
      inputVal: '',
      isChecked: '',
      isShow: false,
      isShowTips: false
    }
  },

  methods: {
    showLogin() {
      this.$store.commit('showRegister', false);
      this.$store.commit('showMsg', false);
    },
    check(ak) {
      if (!this.isChecked) {
        this.isShowTips = !this.isShowTips;
        return;
      }
      if (!ak.trim()) {
        return;
      }
      this.axios.post('https://cnodejs.org/api/v1/accesstoken', {accesstoken: ak})
        .then(result => {
          if (result.status === 200) {
            return result.data;
          }
        })
        .catch(function (error) {
          console.log('验证失败',error);
        })
        .then(userInfo => {
          this.$store.commit('updateUserInfo', userInfo);
          this.$store.commit('updateAk', ak);
          localStorage.userInfo = JSON.stringify(userInfo);
          localStorage.ak = ak;
          this.showLogin();
        })
    },
    goRegister() {
      this.$store.commit('showLogin', true);
      this.$store.commit('showRegister', false);
    },
    showRule() {
      this.isShow = true;
    }
  },

  computed: {
    ak() {
      return this.$store.state.ak;
    }
  },
  mounted() {
    this.inputVal = this.ak;
  }
}
</script>

<style lang="scss" scoped>
  .login {
    position: absolute;
    display: flex;
    flex-direction: column;
    // justify-content: center;
    align-items: center;
    z-index: 2;
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: white;

    .login-header {
      display: flex;
      align-items: center;
      justify-content: center;
      width: 100%;
      height: 60px;
      background-color: #2196f3;
      color: white;
      font-size: 1.4rem;

      i.icon-back {
        position: absolute;
        left: 10px;
        display: inline-block;
        width: 38px;
        height: 38px;
        background: url('../../common/icons/icon-back.svg') no-repeat;
        background-size: contain;
      }
    }

    .input {
      display: flex;
      flex-direction: column;
      align-items: center;
      padding-top: 50px;
      flex: 1;
      width: 100%;
      // background-color: rgba(0, 0, 0, .1);

      input {
        outline: none;
        border: none;
        height: 40px;
        width: 320px;
        // border-radius: 5px;
        border-bottom: 1px solid gray;
        font-size: 1.3rem;
        padding: 0;
      }
      input:focus {
        border-bottom: 1px solid #2962FF;
      }
      &__p {
        padding-top: 10px;
        width: 320px;
        color: #2196f3;
        .input__span {
          color: red;
          font-size: 12px;
        }
      }
      &__checkbox {
        height: auto !important;
        width: auto !important;
      }
      button {
        margin-top: 50px;
        font-size: 1.3rem;
        background-color: #2196f3;
        color: white;
        border: none;
        padding: 5px 10px;
        border-radius: 3px;
        letter-spacing: 3px;
        outline: none;
      }
    }
    .tips {
      flex: 2;
      color: #2196f3;
      // box-shadow: 0 0 10px gray;
      // border: 1px solid gray;
      li {
        list-style: none;
      }
    }

    .mask {
      position: fixed;
      top: 0;
      bottom: 0;
      left: 0;
      right: 0;
      background: rgba(0, 0, 0, .3);
      padding-top: 100px;
      &__content {
        width: 320px;
        max-height: 400px;
        margin: 0 auto;
        padding: 15px;
        background: #fff;
        overflow-y: scroll;
      }
    }
  }
</style>
