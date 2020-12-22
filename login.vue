<template>
  <div class="bg">
    <div class="login-box">
      <div class="logo" @click="goPage('/website/home')">
        <div class="text"></div>
      </div>
      <p class="text">用户登录</p>
      <el-tabs v-model="activeName">
        <el-tab-pane label="手机登录" name="1">
          <div class="error-text" v-if="isError">
            <i class="el-icon-error err"></i>
            {{ errText }}
          </div>
          <div class="line com-sel por">
            <el-select class="selectPhone" v-model="Areacode" placeholder>
              <el-option value="+86">+ 86</el-option>
              <el-option value="+886">+ 886</el-option>
              <el-option value="+852">+ 852</el-option>
              <el-option value="+853">+ 853</el-option>
            </el-select>
            <input
              type="text"
              @input="
                phoneForm.phoneNumber = phoneForm.phoneNumber.replace(
                  /[^\d]/g,
                  ''
                )
              "
              class="input-phone"
              v-model="phoneForm.phoneNumber"
              placeholder="请输入手机号"
            />
          </div>
          <div class="line">
            <input
              type="password"
              @input="
                phoneForm.password = phoneForm.password.replace(
                  /[^0-9a-zA-Z]/g,
                  ''
                )
              "
              class="pass-input"
              v-model="phoneForm.password"
              @focus="needVerificat ? resetSlider() : (showSlider = false)"
              placeholder="请输入您的密码"
              @keyup.enter="userLogin"
            />
          </div>
        </el-tab-pane>
        <el-tab-pane label="邮箱登录" name="2">
          <div class="error-text" v-if="isError">
            <i class="el-icon-error err"></i>
            {{ errText }}
          </div>
          <div class="line">
            <input
              type="text"
              class="pass-input"
              v-model="emailForm.email"
              placeholder="请输入邮箱"
            />
          </div>
          <div class="line">
            <input
              type="password"
              class="pass-input"
              @input="
                emailForm.password = emailForm.password.replace(
                  /[^0-9a-zA-Z]/g,
                  ''
                )
              "
              v-model="emailForm.password"
              @focus="needVerificat ? resetSlider() : (showSlider = false)"
              placeholder="请输入您的密码"
              @keyup.enter="userLogin"
            />
          </div>
        </el-tab-pane>
      </el-tabs>
      <div class="clanm">
        <div class="checkbox-box" @click="rememberMe = !rememberMe">
          <input type="checkbox" v-model="rememberMe" />
          <label>记住密码</label>
        </div>
        <el-button type="text" @click="goPage('/backstage/resetpassword')"
          >忘记密码</el-button
        >
      </div>
      <div class="drag" ref="dragDiv" v-if="showSlider">
        <div class="drag_bg"></div>
        <div class="drag_text">{{ confirmWords }}</div>
        <div
          ref="moveDiv"
          @mousedown="mousedownFn($event)"
          :class="{ handler_ok_bg: confirmSuccess }"
          class="handler handler_bg"
          style="position: absolute; top: 0px; left: 0px; height: 100%"
        ></div>
      </div>
      <el-button type="primary" class="btn" @click="userLogin">登录</el-button>
      <div class="end">
        <el-button type="text" @click="goPage('/register')" class="left"
          >免费注册</el-button
        >
        <el-button type="text" @click="goPage('/website/home')"
          >返回首页</el-button
        >
      </div>
      <el-divider content-position="center">or</el-divider>
      <div class="icon-box">
        其他登录方式：
        <a href="weixin://dl/groupchat" @click="wechatLogin">
          <img src="../../../assets/img/backstage/login/wechat.png" alt />
        </a>
        <!-- <a href="https://graph.qq.com/oauth2.0/show?which=Login&display=pc&client_id=100477069&response_type=code&scope=all&redirect_uri=https%3A%2F%2Fwww.52wmb.com%2Fauthorization%2Fqq%3Fwmb_redirect_uri%3Dhttps%253A%252F%252Fwww.52wmb.com%252F&state=ca19c3f6-af70-11ea-aee4-00155d391e0d" >
        <a @click="goPage('/backstage/bindphone')">
          <img src="../../../assets/img/backstage/login/qq.png" alt />
        </a>
        <a href="https://api.weibo.com/oauth2/authorize?client_id=4075922869&redirect_uri=https%3A%2F%2Fwww.52wmb.com%2Fauthorization%2Fsina%3Fwmb_redirect_uri%3Dhttps%253A%252F%252Fwww.52wmb.com%252F&response_type=code&display=default&state=ec61b492-af70-11ea-9e98-00155d391e0d">
        <a @click="goPage('/backstage/bindphone')">
          <img src="../../../assets/img/backstage/login/weibo.png" alt />
        </a> -->
      </div>
    </div>
    <div class="footer">
      <div class="foot-nav">
        <router-link
          tag="a"
          target="_blank"
          :to="{ path: '/backstage/agreement' }"
        >
          <span class="item">用户协议</span>
        </router-link>
        <router-link
          tag="a"
          target="_blank"
          :to="{ path: '/backstage/aprivacy' }"
        >
          <span>隐私政策</span>
        </router-link>
      </div>
      <div class="copyright">
        copyright©2018-2020 Data-sprite.com版权所有 &nbsp;&nbsp;
        <a class="prepare" href="http://www.beian.miit.gov.cn/" target="_blank"
          >粤ICP备19011644号-2</a
        >&nbsp;&nbsp; 本站由阿里云提供计算和安全服务
      </div>
    </div>
  </div>
</template>


<style lang="scss" scoped>
/deep/ .el-tabs__active-bar {
  height: 3px;
  background-color: #1888ec;
}
/deep/ .el-tabs__item {
  font-weight: bold;
  &:hover,
  &.is-active {
    color: #1888ec;
  }
}
.bg {
  width: 100%;
  min-height: 100%;
  background: url("../../../assets/img/backstage/login/bg.jpg") no-repeat center;
  background-size: 100% 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  .login-box {
    margin-top: 7%;
    width: 475px;
    padding: 0 48px;
    box-sizing: border-box;
    background-color: #fff;
    display: flex;
    align-items: center;
    flex-direction: column;
    position: relative;
    z-index: 1;
    .logo {
      margin-top: 38px;
      background: url("../../../assets/img/backstage/login/logo.png") no-repeat
        center;
      border-radius: 50%;
      width: 120px;
      height: 120px;
      margin-bottom: 10px;
      cursor: pointer;
    }
    .text {
      font-size: 16px;
      font-family: Microsoft YaHei;
      font-weight: 400;
      font-size: 16px;
      margin-bottom: 26px;
      color: rgba(102, 102, 102, 1);
      line-height: 33px;
    }
    .error-text {
      width: 100%;
      height: 28px;
      line-height: 28px;
      font-size: 9px;
      font-weight: normal;
      background-color: #ffefef;
      border: solid 1px #fdc6c6;
      margin-bottom: 22px;
      color: #851913;
      .err {
        color: #dd4040;
        margin-left: 10px;
        margin-right: 8px;
      }
    }
    .clanm {
      display: flex;
      justify-content: space-between;
      width: 380px;
      font-size: 14px;
      margin-bottom: 10px;
      align-items: center;
      .checkbox-box {
        display: flex;
        cursor: pointer;
        align-items: flex-start;
        input[type="checkbox"] {
          width: 14px;
          border-radius: 2px;
          height: 14px;
          vertical-align: sub;
          margin-right: 6px;
          margin-top: 3px;
          background-color: #eeeeee;
          border: 1px solid #d2d2d2;
        }
        label {
          font-size: 14px;
          cursor: pointer;
        }
      }
    }
    .line {
      display: flex;
      justify-content: space-between;
      align-items: center;
      width: 100%;
      height: 45px;
      border-radius: 5px;
      margin-bottom: 23px;
    }
    .com-sel {
      display: flex;
      cursor: pointer; /*鼠标上移变成小手*/
      // border: 1px solid #dcdfe6;
      width: 100%;
      height: 45px;
      border-radius: 5px;
      border-collapse: collapse;
      line-height: 38px;
      .selectPhone {
        width: 77px;
        color: #333333;
        font-size: 14px;
        height: 100%;
        border: none;
        outline: none;
        cursor: pointer;
      }
      .input-phone {
        height: 100%;
        text-indent: 24px;
        border: 1px solid #ccc;
        border-left: none;
        border-top-left-radius: 0;
        border-top-right-radius: 5px;
        border-bottom-left-radius: 0;
        border-bottom-right-radius: 5px;
      }
    }
    /deep/ .el-tabs {
      width: 100%;
      .el-tabs__header {
        width: 100%;
        margin-bottom: 20px;
        .el-tabs__item {
          font-size: 16px;
        }
        .el-tabs__nav-scroll {
          display: flex;
          justify-content: center;
        }
        .el-tabs__nav-wrap::after {
          height: 1px;
          background-color: rgba(255, 255, 255, 0.3);
        }
      }
    }
  }
  .footer {
    width: 100%;
    height: 60px;
    margin-top: 2%;
    margin-bottom: 1%;
    .foot-nav {
      text-align: center;
      cursor: pointer;
      color: #999999;
      line-height: 12px;
      margin: 4px 0;
      margin-bottom: 12px;
      font-size: 13px;
      a {
        color: #999999;
      }
      .item {
        &::after {
          height: calc(100% - 0.8em);
          margin: 0 18px;
          content: "";
          width: 0;
          border-left: solid #999999 1px;
        }
      }
    }
    .copyright {
      font-size: 13px;
      font-weight: normal;
      line-height: 18px;
      color: #999999;
      text-align: center;
    }
  }
}
/deep/ .el-input {
  width: 100%;
  margin-bottom: 20px;
  /deep/ .el-input__inner {
    height: 45px;
    line-height: 45px;
    padding-left: 14px;
    padding-right: 20px;
    border-top-right-radius: 0;
    border-bottom-right-radius: 0;
  }
}
.drag {
  position: relative;
  background-color: #e8e8e8;
  width: 100%;
  height: 34px;
  line-height: 34px;
  text-align: center;
}
.handler {
  width: 40px;
  height: 32px;
  border: 1px solid #ccc;
  cursor: move;
}
.handler_bg {
  background: #fff
    url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAA3hpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNS1jMDIxIDc5LjE1NTc3MiwgMjAxNC8wMS8xMy0xOTo0NDowMCAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wTU09Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9tbS8iIHhtbG5zOnN0UmVmPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvc1R5cGUvUmVzb3VyY2VSZWYjIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtcE1NOk9yaWdpbmFsRG9jdW1lbnRJRD0ieG1wLmRpZDo0ZDhlNWY5My05NmI0LTRlNWQtOGFjYi03ZTY4OGYyMTU2ZTYiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6NTEyNTVEMURGMkVFMTFFNEI5NDBCMjQ2M0ExMDQ1OUYiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6NTEyNTVEMUNGMkVFMTFFNEI5NDBCMjQ2M0ExMDQ1OUYiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTQgKE1hY2ludG9zaCkiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDo2MTc5NzNmZS02OTQxLTQyOTYtYTIwNi02NDI2YTNkOWU5YmUiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6NGQ4ZTVmOTMtOTZiNC00ZTVkLThhY2ItN2U2ODhmMjE1NmU2Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+YiRG4AAAALFJREFUeNpi/P//PwMlgImBQkA9A+bOnfsIiBOxKcInh+yCaCDuByoswaIOpxwjciACFegBqZ1AvBSIS5OTk/8TkmNEjwWgQiUgtQuIjwAxUF3yX3xyGIEIFLwHpKyAWB+I1xGSwxULIGf9A7mQkBwTlhBXAFLHgPgqEAcTkmNCU6AL9d8WII4HOvk3ITkWJAXWUMlOoGQHmsE45ViQ2KuBuASoYC4Wf+OUYxz6mQkgwAAN9mIrUReCXgAAAABJRU5ErkJggg==")
    no-repeat center;
}
.handler_ok_bg {
  background: #fff url("../../../assets/img/backstage/login/checkok.png")
    no-repeat center;
}
.drag_bg {
  background-color: #f78000;
  height: 34px;
  width: 0px;
}
.drag_text {
  position: absolute;
  top: 0px;
  width: 100%;
  text-align: center;
  -moz-user-select: none;
  -webkit-user-select: none;
  user-select: none;
  -o-user-select: none;
  -ms-user-select: none;
  color: #666;
  font-size: 14px;
}
.btn {
  width: 100%;
  height: 45px;
  padding: 0;
  margin-top: 20px;
}
.end {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  width: 100%;
  .left {
    &::after {
      height: calc(100% - 0.2em);
      margin-left: 4px;
      content: "";
      width: 0;
      border-left: solid darkblue 1px;
    }
  }
  /deep/ .el-button + .el-button {
    margin-left: 4px;
  }
}
.icon-box {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  margin-bottom: 25px;
  cursor: pointer;
  img {
    margin-right: 13px;
    vertical-align: middle;
  }
}

input {
  width: 304px;
  height: 45px;
  outline-style: none;
  border: 1px solid #ccc;
  text-indent: 17px;
}
.pass-input {
  width: 380px;
  border-radius: 5px;
}
</style>


<script>
let check = require("@/utils/util").check;
let randomPass = require("@/utils/util").randomPass;
// des加密

let JSEncrypt = require("jsencrypt").JSEncrypt;
let CryptoJS = require("crypto-js");

const mkey = randomPass(16);
const miv = randomPass(16);

const publicKey =
  "MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCGCY4aJoP2folgjL8sQlRWtDpAIpL4hSDbBvO4e5URBmA0k5JgLTNRLRNrzyE/GdYPZS7d4WTt8QTyoQB+qIdyztZT1GP/BLIXKyOf03DuZ20lkwiSW9l58kOZFLf84sgGUBExfGwZeRu5xfRezQV0/MD+hDp/qdV/v+xW2riPgwIDAQAB";

export default {
  metaInfo: {
    title: "登录 - 专注外贸行业大数据分析选品", // set a title
    meta: [
      {
        // set meta
        name: "keyWords",
        content:
          "数据精灵, Alibaba关键词挖掘，Amazon关键词挖掘，关键词趋势对比，Amazon热品挖掘，Amazon爆品榜单，Alibaba国际站标杆同行，选品资讯",
      },
      {
        // set meta
        name: "description",
        content:
          "数据精灵 - 属于广东一鸣信息科技有限公司旗下开发的互联网产品，专注外贸行业大数据分析选品，让中国实业在竞争激烈的外贸市场少走弯路，并脱颖而出！",
      },
    ],
  },
  data() {
    return {
      needVerificat: false,
      Areacode: "+86",
      // 登录方式 1是手机  2是邮箱
      activeName: "1",
      phoneForm: {
        phoneNumber: "",
        password: "",
      },
      emailForm: {
        email: "",
        password: "",
      },
      loginNum: 3,
      showSlider: true,
      isError: false,
      rememberMe: true,
      beginClientX: 0 /*距离屏幕左端距离*/,
      mouseMoveStata: false /*触发拖动状态 判断*/,
      maxwidth: "" /*拖动最大宽度，依据滑块宽度算出来的*/,
      confirmWords: "请按住 滑块，拖动到最右边" /*滑块文字*/,
      confirmSuccess: false /*验证成功判断*/,
    };
  },
  methods: {
    goPage(saveWay) {
      this.$router.push({ path: saveWay });
    },
    wechatLogin() {
      this.$axios.post("/api/vendors/wx-appid").then((res) => {
        window.location.href = `https://open.weixin.qq.com/connect/qrconnect?appid=${res.data.msg}&redirect_uri=${this.$globalUrl}thirdLogin&response_type=code&scope=snsapi_login`;
      });
    },
    // 设置Secret
    fnencrypt(none) {
      let encryptor = new JSEncrypt();
      encryptor.setPublicKey(publicKey);
      let rsaPassWord = encryptor.encrypt(`${mkey}|${miv}|${none}`);
      return rsaPassWord;
    },
    // 加密密码的方法
    Encrypt(password) {
      let mkeys = CryptoJS.enc.Utf8.parse(mkey);
      let mivs = CryptoJS.enc.Utf8.parse(miv);
      let pass = CryptoJS.enc.Utf8.parse(password);
      let encrypted = CryptoJS.AES.encrypt(pass, mkeys, {
        iv: mivs,
        mode: CryptoJS.mode.CBC,
        padding: CryptoJS.pad.Pkcs7,
      });
      return encrypted.toString();
    },
    // 获取记住的密码
    getUser() {
      if (this.getCookie("activeName") == "1") {
        this.activeName = "1";
        this.phoneForm.phoneNumber = this.getCookie("phonenumber");
        this.phoneForm.password = this.getCookie("password");
      } else if (this.getCookie("activeName") == "2") {
        this.activeName = "2";
        this.emailForm.email = this.getCookie("email");
        this.emailForm.password = this.getCookie("password");
      }
      if (this.getCookie("rememberMe"))
        this.rememberMe = this.getCookie("rememberMe");
    },
    //设置cookie
    setCookie(name, value, day) {
      var date = new Date();
      date.setDate(date.getDate() + day);
      document.cookie = name + "=" + value + ";expires=" + date;
    },
    //获取cookie
    getCookie(name) {
      var reg = RegExp(name + "=([^;]+)");
      var arr = document.cookie.match(reg);
      if (arr) {
        return arr[1];
      } else {
        return "";
      }
    },
    //删除cookie
    delCookie(name) {
      this.setCookie(name, null, -1);
    },
    // 验证用户名登陆内容
    checkUserForm() {
      this.isError = false;
      this.errText = "";

      if (this.activeName == "1") {
        if (!check.phoneNumber.test(this.phoneForm.phoneNumber)) {
          this.isError = true;
          this.errText = "请输入正确的手机！";
          return this.isError;
        } else if (
          this.phoneForm.password == null ||
          this.phoneForm.password == ""
        ) {
          this.isError = true;
          this.errText = "请输入密码！";
          return this.isError;
        }
      } else {
        if (!check.email.test(this.emailForm.email)) {
          this.isError = true;
          this.errText = "请输入正确的邮箱！";
          return this.isError;
        } else if (
          this.emailForm.password == null ||
          this.emailForm.password == ""
        ) {
          this.isError = true;
          this.errText = "请输入密码！";
          return this.isError;
        }
      }
    },
    // 用户名称密码登陆
    async userLogin() {
      if (!this.checkUserForm()) {
        if (this.rememberMe) {
          if (this.activeName == "1") {
            let phoneNumber = this.phoneForm.phoneNumber;
            let pwd = this.phoneForm.password;
            this.setCookie("activeName", "1", 30);
            this.setCookie("phonenumber", phoneNumber, 30);
            this.setCookie("password", pwd, 30);
            this.setCookie("rememberMe", this.rememberMe, 30);
          } else {
            let email = this.emailForm.email;
            let pwd = this.emailForm.password;
            this.setCookie("activeName", "2", 30);
            this.setCookie("email", email, 30);
            this.setCookie("password", pwd, 30);
            this.setCookie("rememberMe", this.rememberMe, 30);
          }
        } else {
          if (this.activeName == "1") {
            this.delCookie("phonenumber");
            this.delCookie("password");
            this.delCookie("rememberMe");
          } else {
            this.delCookie("email");
            this.delCookie("password");
            this.delCookie("rememberMe");
          }
        }

        this.$store.commit("changeTips", true);

        if (this.loginNum == 1) {
          this.needVerificat = true;
          this.resetSlider();
        }

        if (this.showSlider && !this.confirmSuccess) {
          this.isError = true;
          this.errText = "请拖动滑块";
          this.loginNum = 0;
          return;
        } else {
          this.showSlider = false;
        }

        await this.$axios.post("/api/secure/nonce").then((res) => {
          if (res.data.code == 0) {
            let nonce = this.fnencrypt(res.data.data);
            this.$store.commit("saveSecret", nonce);
          }
        });

        let json;

        if (this.activeName == "1") {
          json = {
            mobile: this.phoneForm.phoneNumber
              .toLowerCase()
              .replace(/\s+/g, ""),
            mobileArea: this.Areacode,
            password: this.Encrypt(this.phoneForm.password.replace(/\s+/g, "")),
            type: this.activeName,
          };
        } else {
          json = {
            email: this.emailForm.email,
            password: this.Encrypt(this.emailForm.password.replace(/\s+/g, "")),
            type: this.activeName,
          };
        }

        await this.$axios.post("/api/login", json).then((res) => {
          if (res.data.code === 0) {
            this.$store.commit("saveUserinfo", res.data.data);
            this.$store.commit("loginWay", true);
            this.$store.commit("changeTab", "0");
            this.$router.push({ path: "/backstage/usercenter" });
            this.$store.commit("changeTips", true);
            this.$store.commit("isBindweacht", false);
          } else {
            this.isError = true;
            this.errText = res.data.msg;
            this.loginNum -= 1;
          }
        });
      }
    },

    resetSlider() {
      this.showSlider = true;
      this.confirmSuccess = false;
      this.confirmWords = "请按住 滑块，拖动到最右边";
      this.$nextTick(() => {
        this.maxwidth =
          this.$refs.dragDiv.clientWidth - this.$refs.moveDiv.clientWidth;
        document
          .getElementsByTagName("html")[0]
          .addEventListener("mousemove", this.mouseMoveFn);
        document
          .getElementsByTagName("html")[0]
          .addEventListener("mouseup", this.moseUpFn);
      });
    },
    mousedownFn: function (e) {
      //mousedoen 事件
      if (!this.confirmSuccess) {
        e.preventDefault && e.preventDefault(); //阻止文字选中等 浏览器默认事件
        this.mouseMoveStata = true;
        this.beginClientX = e.clientX;
      }
    },
    successFunction() {
      //验证成功函数
      this.confirmSuccess = true;
      this.confirmWords = "验证通过";
      if (window.addEventListener) {
        document
          .getElementsByTagName("html")[0]
          .removeEventListener("mousemove", this.mouseMoveFn);
        document
          .getElementsByTagName("html")[0]
          .removeEventListener("mouseup", this.moseUpFn);
      } else {
        document
          .getElementsByTagName("html")[0]
          .removeEventListener("mouseup", () => {});
      }
      document.getElementsByClassName("drag_text")[0].style.color = "#fff";
      document.getElementsByClassName("handler")[0].style.left =
        this.maxwidth + "px";
      document.getElementsByClassName("drag_bg")[0].style.width =
        this.maxwidth + "px";
    },
    mouseMoveFn(e) {
      //mousemove事件
      if (this.mouseMoveStata) {
        let width = e.clientX - this.beginClientX;
        if (width > 0 && width <= this.maxwidth) {
          document.getElementsByClassName("handler")[0].style.left =
            width + "px";
          document.getElementsByClassName("drag_bg")[0].style.width =
            width + "px";
        } else if (width > this.maxwidth) {
          this.successFunction();
        }
      }
    },
    moseUpFn(e) {
      //mouseup事件
      this.mouseMoveStata = false;
      var width = e.clientX - this.beginClientX;
      if (width < this.maxwidth) {
        document.getElementsByClassName("handler")[0].style.left = 0 + "px";
        document.getElementsByClassName("drag_bg")[0].style.width = 0 + "px";
      }
    },
  },
  created() {
    this.getUser();
  },
};
</script>


