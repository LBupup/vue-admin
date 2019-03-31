<template>
  <div id="app">
    <!-- 头部导航 -->
    <header class="header" :class="{ 'header-fixed' : headerFixed }">
      <el-row>
        <el-col :span="24">
          <el-menu default-active="2" class="el-menu-demo" mode="horizontal" select>
            <el-menu-item index="1">工作台</el-menu-item>
            <el-menu-item index="2">综合管理</el-menu-item>
            <el-menu-item index="3">工程管理</el-menu-item>
            <el-menu-item index="4">科技档案</el-menu-item>
            <el-menu-item index="5">项目评优</el-menu-item>
            <el-menu-item index="6">资源仓</el-menu-item>
          </el-menu>
        </el-col>
      </el-row>
    </header>
    <div v-show="headerFixed" style="position: relative;height: 60px;width: 100%;"></div>

    <main>
      <!-- 左侧导航 -->
      <div class="main-left">
        <el-tabs default-active="/activePublic" class="el-menu-vertical-demo" :router="true">
          <el-tab-pane class="tab-title" label="案卷管理" index="/activePublic">
            <el-input class="elInput" placeholder="请输入关键字" v-model="filterText"></el-input>
            <el-tree
              class="filter-tree"
              :data="data2"
              :props="defaultProps"
              default-expand-all
              :filter-node-method="filterNode"
              ref="tree2"
            ></el-tree>
          </el-tab-pane>
          <el-tab-pane class="tab-title" label="文件管理" index="/activeManage">
            <el-input class="elInput" placeholder="请输入关键字" v-model="filterText"></el-input>
            <el-tree
              class="filter-tree"
              :data="data2"
              :props="defaultProps"
              default-expand-all
              :filter-node-method="filterNode"
              ref="tree2"
            ></el-tree>
          </el-tab-pane>
        </el-tabs>
      </div>

      <!-- 右侧主内容区 -->
      <div class="main-right">
        <transition name="fade">
          <router-view class="view"></router-view>
        </transition>
      </div>
    </main>
  </div>
</template>

<script>
import Vue from "vue";
import Element from "element-ui";
import "element-ui/lib/theme-default/index.css";
import $ from "jquery";

Vue.use(Element);

export default {
  name: "app",
  data: function() {
    return {
      active: true,
      headerFixed: true,
      filterText: "",
      data2: [
        {
          id: 1,
          label: "一级 1",
          children: [
            {
              id: 4,
              label: "二级 1-1",
              children: [
                {
                  id: 9,
                  label: "三级 1-1-1"
                },
                {
                  id: 10,
                  label: "三级 1-1-2"
                }
              ]
            }
          ]
        },
        {
          id: 2,
          label: "一级 2",
          children: [
            {
              id: 5,
              label: "二级 2-1"
            },
            {
              id: 6,
              label: "二级 2-2"
            }
          ]
        },
        {
          id: 3,
          label: "一级 3",
          children: [
            {
              id: 7,
              label: "二级 3-1"
            },
            {
              id: 8,
              label: "二级 3-2"
            }
          ]
        }
      ],
      defaultProps: {
        children: "children",
        label: "label"
      }
    };
  },
  created: function() {
    this.$router.push("/activePublic");
  },
  methods: {
    filterNode(value, data) {
      if (!value) return true;
      return data.label.indexOf(value) !== -1;
    }
  },
  watch: {
    $route: function(to, from) {
      if (to.path == "/activePublic") {
        this.active = true;
      } else if (to.path == "/activeManage") {
        this.active = false;
      }
    },
    filterText(val) {
      this.$refs.tree2.filter(val);
    }
  }
};
</script>

<style scoped>
body {
  margin: 0;
}
#app {
  min-width: 1200px;
  margin: 0 auto;
  font-family: "Helvetica Neue", "PingFang SC", Arial, sans-serif;
}
/* 头部导航 */
header {
  z-index: 1000;
  min-width: 1200px;
  transition: all 0.5s ease;
  border-top: solid 4px #3091f2;
  background-color: #fff;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.12), 0 0 6px 0 rgba(0, 0, 0, 0.04);
}
header.header-fixed {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
}
header .el-menu-demo {
  padding-left: 300px !important;
}

/* 主内容区 */
main {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  min-height: 800px;
  border: solid 40px #e9ecf1;
  background-color: #fcfcfc;
}
main .main-left {
  text-align: center;
  -webkit-box-flex: 0;
  -ms-flex: 0 0 200px;
  flex: 0 0 200px;
}
main .main-right {
  -webkit-box-flex: 1;
  -ms-flex: 1;
  flex: 1;
  background-color: #fff;
  padding: 50px 70px;
}
main .el-menu {
  background-color: transparent !important;
}
/*  */
.router-link {
  display: inline-block;
  width: 100%;
  height: 100%;
  text-align: center;
  color: #475669;
  text-decoration: none;
}
.is-active .router-link {
  color: #20a0ff;
}

/* 单选框 */
.el-form-item .el-radio + .el-radio {
  margin-left: 30px !important;
}
/* 路由切换动效 */
.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s;
}
.fade-enter,
.fade-leave-active {
  opacity: 0;
}

.list-enter-active,
.list-leave-active {
  transition: all 1s;
}
.list-enter,
.list-leave-active {
  opacity: 0;
  transform: translateY(30px);
}

/* 导航栏菜单选中效果 */
.isActive {
  color: #20a0ff !important;
}
#app main .aside .is-active {
  color: #475669;
}

/* 卡片 */
.el-card {
  overflow: visible !important;
}
.filter-tree {
  text-align: left;
  padding: 0 5px;
}
.el-tabs__item {
  font-size: 22px !important;
}
</style>

