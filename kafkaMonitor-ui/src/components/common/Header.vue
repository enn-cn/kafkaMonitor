<template>
  <div class="header">
    <!-- 折叠按钮 -->
    <div class="collapse-btn" @click="collapseChage">
      <i class="el-icon-menu"></i>
    </div>
    <div class="logo">KafkaMonitor</div>
    <div class="header-right">
      <div class="header-user-con">
        <!-- 全屏显示 -->
        <div class="btn-fullscreen" @click="handleFullScreen">
          <el-tooltip effect="dark" :content="fullscreen?`取消全屏`:`全屏`" placement="bottom">
            <i class="el-icon-rank"></i>
          </el-tooltip>
        </div>
        <!-- 消息中心 -->
        <!--        <div class="btn-bell">-->
        <!--          <el-tooltip effect="dark" :content="message?`有${message}条未读消息`:`消息中心`" placement="bottom">-->
        <!--            <el-badge :value="message" class="item">-->
        <!--              <router-link to="/tabs">-->
        <!--                <i class="el-icon-bell"></i>-->
        <!--              </router-link>-->
        <!--            </el-badge>-->
        <!--          </el-tooltip>-->
        <!--        </div>-->
        <!-- 用户头像 -->
<!--        <div class="user-avator"><img src="../../assets/img/img.jpg"></div>-->
        <!-- 用户名下拉菜单 -->
        <el-dropdown class="user-name" @command="handleCommand">
                    <span class="el-dropdown-link">
                        {{name}} <i class="el-icon-arrow-down el-icon--right"></i>
                    </span>
          <el-dropdown-menu slot="dropdown">
<!--            <el-dropdown-item divided command="userCenter">用户中心</el-dropdown-item>-->
            <el-dropdown-item divided command="loginOut">退出登录</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>

      </div>
    </div>
  </div>
</template>

<script>
  import {loginOut} from '../../api/UserApi'
  import {getCookie, setCookie, delCookie} from '../../util/cookieUtil'

  export default {
    data() {
      return {
        message: 2,
        activeIndex: '1',
        fullscreen: false,
        // name: this.$store.state.name,
        // username: this.$store.state.username
      }
    },
    methods: {
      collapseChage() {
        this.$store.commit('changeCollapse');
      },
      // 用户名下拉菜单选择事件
      handleCommand(command) {
        if (command === 'loginOut') {
          // localStorage.removeItem('loginUserBaseInfo');
          loginOut().then(response => {
            let data = response.data;
            if (data.code === 403) {
              delCookie('token');
              this.$router.push('/login');
            }
          });
        }
        if (command === 'userCenter') {
          this.$router.push('/user');
        }
      },
      // 全屏事件
      handleFullScreen() {
        let element = document.documentElement;
        if (this.fullscreen) {
          if (document.exitFullscreen) {
            document.exitFullscreen();
          } else if (document.webkitCancelFullScreen) {
            document.webkitCancelFullScreen();
          } else if (document.mozCancelFullScreen) {
            document.mozCancelFullScreen();
          } else if (document.msExitFullscreen) {
            document.msExitFullscreen();
          }
        } else {
          if (element.requestFullscreen) {
            element.requestFullscreen();
          } else if (element.webkitRequestFullScreen) {
            element.webkitRequestFullScreen();
          } else if (element.mozRequestFullScreen) {
            element.mozRequestFullScreen();
          } else if (element.msRequestFullscreen) {
            // IE11
            element.msRequestFullscreen();
          }
        }
        this.fullscreen = !this.fullscreen;
      }
    },
    mounted() {
      if (document.body.clientWidth < 1500) {
        this.collapseChage();
      }
    },
    computed: {
      collapse() {
        return this.$store.state.collapse
      },
      name() {
        return  this.$store.state.name;
        // let name = getCookie('name');
        // return name ? name : this.name;
      },
      username() {
        return  this.$store.state.username;
        // let username = getCookie('username');
        // return username ? username : this.username;
      }
    }
  }
</script>

<style scoped>
  .header {
    position: relative;
    box-sizing: border-box;
    width: 100%;
    height: 50px;
    font-size: 22px;
    color: #fff;
    child-align: right;
  }

  .collapse-btn {
    float: left;
    padding: 0 21px;
    cursor: pointer;
    line-height: 50px;
  }

  .header .logo {
    float: left;
    width: 250px;
    line-height: 50px;
  }

  .header-right {
    float: right;
    padding-right: 30px;
  }

  .header-user-con {
    display: flex;
    height: 60px;
    align-items: center;
  }

  .btn-fullscreen {
    transform: rotate(45deg);
    margin-right: 5px;
    margin-bottom: 10px;
    font-size: 24px;
  }

  .btn-bell, .btn-fullscreen {
    position: relative;
    width: 40px;
    height: 30px;
    text-align: center;
    border-radius: 10px;
    cursor: pointer;
  }

  .btn-bell-badge {
    position: absolute;
    right: 0;
    top: -2px;
    width: 8px;
    height: 8px;
    border-radius: 4px;
    background: #f56c6c;
    color: #fff;
  }

  .btn-bell .el-icon-bell {
    color: #fff;
  }

  .user-name {
    margin-left: 5px;
    margin-bottom: 5px;
  }

  .user-avator {
    margin-left: 20px;
    margin-bottom: 10px;
  }

  .user-avator img {
    display: block;
    width: 40px;
    height: 40px;
    border-radius: 50%;
  }

  .el-dropdown-link {
    color: #fff;
    cursor: pointer;
  }

  .el-dropdown-menu__item {
    text-align: center;
  }

</style>
