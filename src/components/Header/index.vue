<template>
  <header class="header">
    <!-- 头部的第一行 -->
    <div class="top">
      <div class="container">
        <div class="loginList">
          <p>尚品汇欢迎您！</p>
          <!-- 如果没有用户名（用户还没有注册—） -->
          <p v-if="!userName">
            <span>请</span>
            <router-link to="/login">登录</router-link>
            <router-link to="/register"
                         class="register">免费注册</router-link>
          </p>
          <!-- 如果用户已经注册了 -->
          <p v-else>
            <a>{{userName}}</a>
            <a class="register"
               @click="logout">退出登录</a>
          </p>
        </div>
        <div class="typeList">
          <router-link to="/center/myorder">我的订单</router-link>
          <router-link to="/shopcart">我的购物车</router-link>
          <a href="javascript:;">我的尚品汇</a>
          <a href="javascript:;">尚品汇会员</a>
          <a href="javascript:;">企业采购</a>
          <a href="javascript:;">关注尚品汇</a>
          <a href="javascript:;">合作招商</a>
          <a href="javascript:;">商家后台</a>
        </div>
      </div>
    </div>
    <!--头部第二行 搜索区域-->
    <div class="bottom">
      <h1 class="logoArea">
        <router-link class="logo"
                     to="/home">
          <img src="./images/logo.png"
               alt="" />
        </router-link>
      </h1>
      <div class="searchArea">
        <form action="###"
              class="searchForm">
          <input type="text"
                 id="autocomplete"
                 class="input-error input-xxlarge"
                 v-model="keyword" />
          <button class="sui-btn btn-xlarge btn-danger"
                  type="button"
                  @click="goSearch">
            搜索
          </button>
        </form>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: '',
  data() {
    return {
      keyword: ''
    };
  },
  computed: {
    userName() {
      return this.$store.state.user.userInfo.name;
    }
  },
  methods: {
    goSearch() {
      //3种路由传递参数方式：字符串 + 模板字符串 + 对象写法
      //   this.$router.push('/search/' + this.keyword + '?k=' + this.keyword);
      //   this.$router.push(`/search/${this.keyword}?k=${this.keyword}`);
      //判断有无query，有就一起带过去
      if (this.$route.query) {
        let location = {
          name: 'search',
          //params首先需要在路由配置文件里占位
          params: { keyword: this.keyword || undefined }
        };
        location.query = this.$route.query;
        this.$router.push(location);
      }
    },
    async logout() {
      try {
        await this.$store.dispatch('logout');
        //如果退出成功，跳转到首页
        this.$router.push('/home');
      } catch (error) {
        alert(error.message);
      }
    }
  },
  mounted() {
    //通过全局事件总线清除关键字 - 接受通知
    this.$bus.$on('clear', () => {
      this.keyword = '';
    });
  }
  // watch: {
  //   $route(newValue, oldValue) {
  //     this.keyword = '';
  //   }
  // }
};
</script>

<style scpoed lang="less">
.header {
  & > .top {
    background-color: #eaeaea;
    height: 30px;
    line-height: 30px;

    .container {
      width: 1200px;
      margin: 0 auto;
      overflow: hidden;

      .loginList {
        float: left;

        p {
          float: left;
          margin-right: 10px;

          .register {
            border-left: 1px solid #b3aeae;
            padding: 0 5px;
            margin-left: 5px;
          }
        }
      }

      .typeList {
        float: right;

        a {
          padding: 0 10px;

          & + a {
            border-left: 1px solid #b3aeae;
          }
        }
      }
    }
  }

  & > .bottom {
    width: 1200px;
    margin: 0 auto;
    overflow: hidden;

    .logoArea {
      float: left;

      .logo {
        img {
          width: 175px;
          margin: 25px 45px;
        }
      }
    }

    .searchArea {
      float: right;
      margin-top: 35px;

      .searchForm {
        overflow: hidden;

        input {
          box-sizing: border-box;
          width: 490px;
          height: 32px;
          padding: 0px 4px;
          border: 2px solid #ea4a36;
          float: left;

          &:focus {
            outline: none;
          }
        }

        button {
          height: 32px;
          width: 68px;
          background-color: #ea4a36;
          border: none;
          color: #fff;
          float: left;
          cursor: pointer;

          &:focus {
            outline: none;
          }
        }
      }
    }
  }
}
</style>