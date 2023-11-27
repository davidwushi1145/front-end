<template>
  <div id="Header">
    <div class="top-left-edition">
      <span style="color: red; font-weight: bold">
        >工地安全检测
      </span>
      <span>
        <i class="el-icon-time" style="font-size: 23px"></i>{{ currentTime }}
      </span>
    </div>
    <div id="word">
      <h1>{{ msg }}</h1>
    </div>
  </div>
</template>
<script>

export default {
  name: "Header",
  data() {
    return {
      msg: "工地安全检测系统",
      activeIndex: "1",
      currentTime: new Date().toLocaleTimeString(), // 初始化当前时间
    };
  },
  methods: {},
  created() {
    this.interval = setInterval(() => {
      this.currentTime = new Date().toLocaleTimeString(); // 每秒更新时间
    }, 1000);
  },
  beforeDestroy() {
    clearInterval(this.interval); // 清除定时器
  },
};
</script>
<style scoped>
#Header {
  padding: 30px 110px 0 150px;
  width: 90%;
  margin: 10px auto;
}

#word {
  margin-left: 45%;
  margin-top: -35px;
  margin-bottom: 37px;
  height: 60px;
  line-height: 3.2em;
  letter-spacing: 8px;
}

h1 {
  /*text-align: center;*/
  color: black;
  letter-spacing: 20px;
  font-size: 2.3em;
}

.el-menu-demo {
  width: 80%;
  margin: 0px auto;
  padding: 0px ;
}

.top-left-edition span i {
  float: left;
  margin-right: 10px;
}

i,
input,
label {
  vertical-align: middle;
}

i {
  border: 0;
  display: block;
  cursor: pointer;
}

.top-left-edition span {
  float: left;
  font-size: 16px;
  color: #999999;
  line-height: 24px;
  margin-right: 40px;
}
</style>


