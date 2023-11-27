<template>
  <div id="Content">
    <el-dialog
        :append-to-body="true"
        :center="true"
        :close-on-click-modal="false"
        :close-on-press-escape="false"
        :show-close="false"
        :visible.sync="dialogTableVisible"
        title="AI预测中"
    >
      <el-progress :percentage="percentage"></el-progress>
      <span slot="footer" class="dialog-footer">请耐心等待约3秒钟</span>
    </el-dialog>

    <div id="CT">
      <div id="CT_image">
        <el-tabs id="CT_image_1"
                 class="box-card"
                 style="
            border-radius: 8px;
            width: 890px;
            height: 420px;
            margin-bottom: -30px;
          "
                 type="border-card">
          <el-tab-pane label="安全帽检测">
            <el-card
                id="CT_image_1"
                class="box-card"
                style="
            border-radius: 8px;
            width: 790px;
            height: 360px;
            margin-bottom: -30px;
          "
            >
              <div class="demo-image__preview1">
                <div
                    v-loading="loading"
                    element-loading-spinner="el-icon-loading"
                    element-loading-text="上传图片中"
                >
                  <el-image
                      :preview-src-list="srcList"
                      :src="url_1"
                      class="image_1"
                      style="border-radius: 3px 3px 0 0"
                  >
                    <div slot="error">
                      <div slot="placeholder" class="error">
                        <el-button
                            v-show="showbutton"
                            class="download_bt"
                            icon="el-icon-upload"
                            type="primary"
                            v-on:click="true_upload2"
                        >
                          上传图像
                          <input
                              ref="upload2"
                              name="file"
                              style="display: none"
                              type="file"
                              @change="update"
                          />
                        </el-button>
                      </div>
                    </div>
                  </el-image>
                </div>
                <div class="img_info_1" style="border-radius: 0 0 5px 5px">
                  <span style="color: white; letter-spacing: 6px">原始图像</span>
                </div>
              </div>
              <div class="demo-image__preview2">
                <div
                    v-loading="loading"
                    element-loading-spinner="el-icon-loading"
                    element-loading-text="处理中,请耐心等待"
                >
                  <el-image
                      :preview-src-list="srcList1"
                      :src="url_2"
                      class="image_1"
                      style="border-radius: 3px 3px 0 0"
                  >
                    <div slot="error">
                      <div slot="placeholder" class="error">{{ wait_return }}</div>
                    </div>
                  </el-image>
                </div>
                <div class="img_info_1" style="border-radius: 0 0 5px 5px">
                  <span style="color: white; letter-spacing: 4px">检测结果</span>
                </div>
              </div>
            </el-card>
          </el-tab-pane>
          <el-tab-pane label="危险区域检测">
            <el-card
                id="CT_image_1"
                class="box-card"
                style="
            border-radius: 8px;
            width: 790px;
            height: 360px;
            margin-bottom: -30px;
          "
            >
              <div class="demo-image__preview1">
                <div
                    v-loading="loading"
                    element-loading-spinner="el-icon-loading"
                    element-loading-text="上传图片中"
                >
                  <el-image
                      :preview-src-list="srcList"
                      :src="url_1"
                      class="image_1"
                      style="border-radius: 3px 3px 0 0"
                  >
                    <div slot="error">
                      <div slot="placeholder" class="error">
                        <el-button
                            v-show="showbutton"
                            class="download_bt"
                            icon="el-icon-upload"
                            type="primary"
                            v-on:click="true_upload"
                        >
                          上传图像
                          <input
                              ref="upload"
                              name="file"
                              style="display: none"
                              type="file"
                              @change="up"
                          />
                        </el-button>
                      </div>
                    </div>
                  </el-image>
                </div>
                <div class="img_info_1" style="border-radius: 0 0 5px 5px">
                  <span style="color: white; letter-spacing: 6px">原始图像</span>
                </div>
              </div>
              <div class="demo-image__preview2">
                <div
                    v-loading="loading"
                    element-loading-spinner="el-icon-loading"
                    element-loading-text="处理中,请耐心等待"
                >
                  <el-image
                      :preview-src-list="srcList1"
                      :src="url_2"
                      class="image_1"
                      style="border-radius: 3px 3px 0 0"
                  >
                    <div slot="error">
                      <div slot="placeholder" class="error">{{ wait_return }}</div>
                    </div>
                  </el-image>
                </div>
                <div class="img_info_1" style="border-radius: 0 0 5px 5px">
                  <span style="color: white; letter-spacing: 4px">检测结果</span>
                </div>
              </div>
            </el-card>
          </el-tab-pane>
          <el-tab-pane label="视频检测">
            <el-button type="primary">主要按钮</el-button>
          </el-tab-pane>
        </el-tabs>

      </div>
      <div id="info_patient">
        <!-- 卡片放置表格 -->
        <el-card style="border-radius: 8px">
          <div slot="header" class="clearfix">
            <span>检测目标</span>
            <el-button
                v-show="!showbutton"
                class="download_bt"
                icon="el-icon-upload"
                style="margin-left: 35px"
                type="primary"
                v-on:click="true_upload2"
            >
              重新选择图像
              <input
                  ref="upload2"
                  name="file"
                  style="display: none"
                  type="file"
                  @change="update"
              />
            </el-button>
          </div>
          <el-tabs v-model="activeName">
            <el-tab-pane label="检测到的目标" name="first">
              <!-- 表格存放特征值 -->
              <!-- 表格存放特征值 -->
              <el-table
                  v-loading="loading"
                  :data="feature_list"
                  border
                  element-loading-spinner="el-icon-loading"
                  element-loading-text="数据正在处理中，请耐心等待"
                  height="390"
                  lazy
                  style="width: 750px; text-align: center"
              >
                <el-table-column label="目标类别" prop="class" width="250px"></el-table-column>
                <el-table-column label="目标大小" prop="bbox" width="250px"></el-table-column>
                <el-table-column label="置信度" prop="confidence" width="250px"></el-table-column>
              </el-table>

            </el-tab-pane>
          </el-tabs>
        </el-card>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Content",
  data() {
    return {
      server_url: "http://127.0.0.1:5000",
      activeName: "first",
      active: 0,
      centerDialogVisible: true,
      loadingFinished: false,
      url_1: "",
      url_2: "",
      textarea: "",
      srcList: [],
      srcList1: [],
      feature_list: [],
      feature_list_1: [],
      feat_list: [],
      url: "",
      visible: false,
      wait_return: "等待上传",
      wait_upload: "等待上传",
      loading: false,
      table: false,
      isNav: false,
      showbutton: true,
      percentage: 0,
      fullscreenLoading: false,
      opacitys: {
        opacity: 0,
      },
      dialogTableVisible: false,
      timeout: null,
    };
  },
  created: function () {
    document.title = "工地安全检测系统";
    this.myFunc();
  },
  beforeDestroy() {
    // 组件销毁前清除定时器
    if (this.timeout) {
      clearTimeout(this.timeout);
    }
  },
  methods: {
    true_upload() {
      this.$refs.upload.click();
    },
    true_upload2() {
      this.$refs.upload2.click();
    },
    next() {
      this.active++;
    },
    // 获得目标文件
    getObjectURL(file) {
      var url = null;
      if (window.createObjcectURL !== undefined) {
        url = window.createOjcectURL(file);
      } else if (window.URL !== undefined) {
        url = window.URL.createObjectURL(file);
      } else if (window.webkitURL !== undefined) {
        url = window.webkitURL.createObjectURL(file);
      }
      return url;
    },
    // 上传文件
    update(e) {
      this.loadingFinished = false;
      this.percentage = 0;
      this.dialogTableVisible = true;
      this.url_1 = "";
      this.url_2 = "";
      this.srcList = [];
      this.srcList1 = [];
      this.wait_return = "";
      this.wait_upload = "";
      this.feature_list = [];
      this.feat_list = [];
      this.fullscreenLoading = true;
      this.loading = true;
      this.showbutton = false;
      let file = e.target.files[0];
      this.url_1 = this.$options.methods.getObjectURL(file);
      let param = new FormData(); //创建form对象
      param.append("file", file, file.name); //通过append向form对象添加数据
      var timer = setInterval(() => {
        this.myFunc();
      }, 300);
      let config = {
        headers: {"Content-Type": "multipart/form-data"},
      }; //添加请求头
      axios
          .post(this.server_url + "/upload", param, config)
          .then((response) => {
            this.percentage = 100;
            clearInterval(timer);
            this.url_1 = response.data.image_url;
            this.srcList.push(this.url_1);
            this.url_2 = response.data.draw_url;
            this.srcList1.push(this.url_2);
            this.fullscreenLoading = false;
            this.loading = false;


            // 从响应中获取 image_info 字段
            let imageInfo = response.data.image_info;

// 使用正则表达式匹配所有的字典字符串
            let detectionStrings = imageInfo.match(/{'class':.*?}/g);

// 确保 feature_list 是一个空数组，以便存储解析后的数据
            this.feature_list = [];

// 检查是否有匹配的字符串
            if (detectionStrings) {
              detectionStrings.forEach(jsonString => {
                try {
                  // 将 Python 字典格式的字符串转换为有效的 JSON 字符串
                  let validJsonString = jsonString
                      .replace(/'/g, '"') // 替换所有单引号为双引号
                      .replace(/False/g, 'false') // 将 Python 的 False 替换为 JavaScript 的 false
                      .replace(/True/g, 'true'); // 将 Python 的 True 替换为 JavaScript 的 true

                  // 将 JSON 字符串解析为对象
                  let detection = JSON.parse(validJsonString);

                  // 构造一个新对象，包含类别、大小（边界框）和置信度
                  let feature = {
                    class: detection.class, // 目标类别
                    bbox: detection.bbox.join(', '), // 将边界框数组转换为字符串
                    confidence: detection.confidence.toFixed(2) // 置信度保留两位小数
                  };

                  // 将新对象添加到 feature_list 数组中
                  this.feature_list.push(feature);
                  // 在数据处理逻辑的最后
                  // eslint-disable-next-line no-console
                  console.log(this.feature_list);

                } catch (error) {
                  // 如果解析出错，可以在这里处理错误
                  // eslint-disable-next-line no-console
                  console.error('Parsing error:', error);
                }
              });
              this.loadingFinished = true;
            }

// 设置其他属性
            this.feature_list_1 = this.feature_list.length > 0 ? this.feature_list[0] : null;
            this.dialogTableVisible = false;
            this.percentage = 0;


            this.notice1();
          });
    },
    up(e) {
      this.loadingFinished = false;
      this.percentage = 0;
      this.dialogTableVisible = true;
      this.url_1 = "";
      this.url_2 = "";
      this.srcList = [];
      this.srcList1 = [];
      this.wait_return = "";
      this.wait_upload = "";
      this.feature_list = [];
      this.feat_list = [];
      this.fullscreenLoading = true;
      this.loading = true;
      this.showbutton = false;
      let file = e.target.files[0];
      this.url_1 = this.$options.methods.getObjectURL(file);
      let param = new FormData(); //创建form对象
      param.append("file", file, file.name); //通过append向form对象添加数据
      var timer = setInterval(() => {
        this.myFunc();
      }, 300);
      let config = {
        headers: {"Content-Type": "multipart/form-data"},
      }; //添加请求头
      axios
          .post(this.server_url + "/up", param, config)
          .then((response) => {
            this.percentage = 100;
            clearInterval(timer);
            this.url_1 = response.data.image_url;
            this.srcList.push(this.url_1);
            this.url_2 = response.data.draw_url;
            this.srcList1.push(this.url_2);
            this.fullscreenLoading = false;
            this.loading = false;


            // 从响应中获取 image_info 字段
            let imageInfo = response.data.image_info;

// 使用正则表达式匹配所有的字典字符串
            let detectionStrings = imageInfo.match(/{'class':.*?}/g);

// 确保 feature_list 是一个空数组，以便存储解析后的数据
            this.feature_list = [];

// 检查是否有匹配的字符串
            if (detectionStrings) {
              detectionStrings.forEach(jsonString => {
                try {
                  // 将 Python 字典格式的字符串转换为有效的 JSON 字符串
                  let validJsonString = jsonString
                      .replace(/'/g, '"') // 替换所有单引号为双引号
                      .replace(/False/g, 'false') // 将 Python 的 False 替换为 JavaScript 的 false
                      .replace(/True/g, 'true'); // 将 Python 的 True 替换为 JavaScript 的 true

                  // 将 JSON 字符串解析为对象
                  let detection = JSON.parse(validJsonString);

                  // 构造一个新对象，包含类别、大小（边界框）和置信度
                  let feature = {
                    class: detection.class, // 目标类别
                    bbox: detection.bbox.join(', '), // 将边界框数组转换为字符串
                    confidence: detection.confidence.toFixed(2) // 置信度保留两位小数
                  };

                  // 将新对象添加到 feature_list 数组中
                  this.feature_list.push(feature);
                  // 在数据处理逻辑的最后
                  // eslint-disable-next-line no-console
                  console.log(this.feature_list);

                } catch (error) {
                  // 如果解析出错，可以在这里处理错误
                  // eslint-disable-next-line no-console
                  console.error('Parsing error:', error);
                }
              });
              this.loadingFinished = true;
            }

// 设置其他属性
            this.feature_list_1 = this.feature_list.length > 0 ? this.feature_list[0] : null;
            this.dialogTableVisible = false;
            this.percentage = 0;


            this.notice1();
            // alert("up")
          });
    },
    myFunc() {
      // 如果加载已经完成，直接将进度设置为 100%
      if (this.loadingFinished) {
        this.percentage = 100;
      } else {
        // 如果进度条已经接近完成，减慢增长速度
        const increment = this.percentage > 90 ? Math.random() * 2 + 1 : Math.random() * 5 + 5;
        this.percentage = Math.min(this.percentage + increment, 99.99);
        this.percentage = parseFloat(this.percentage.toFixed(1)); // 保留两位小数

        // 如果进度条还未完成，继续增加
        if (this.percentage < 99.9) {
          const delay = 200 + Math.random() * 1000; // 随机延迟时间
          this.timeout = setTimeout(this.myFunc, delay);
        }
      }
    },

    drawChart() {
    },
    notice1() {
      this.$notify({
        title: "预测成功",
        message: "点击图片可以查看大图",
        duration: 5000,
        type: "success",
      });
    },
  },
  mounted() {
    this.drawChart();
  },
};
</script>

<style>

#hello p {
  font-size: 15px !important;
  /*line-height: 25px;*/
}

</style>

<style scoped>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}

.clearfix:after {
  clear: both;
}

.box-card {
  width: 680px;
  height: 200px;
  border-radius: 8px;
  margin-top: -20px;
}

#CT {
  display: flex;
  height: 100%;
  width: 100%;
  flex-wrap: wrap;
  justify-content: center;
  margin: 0 0 0 auto;
  max-width: 1800px;
}

#CT_image_1 {
  width: 90%;
  height: 40%;
  padding: 0 0;
  margin: 0px 180px 0px auto;
  border-radius: 4px;
}

#CT_image {
  margin-bottom: 60px;
  margin-left: 30px;
  margin-top: 5px;
}

.image_1 {
  width: 275px;
  height: 260px;
  background: #ffffff;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
}

.img_info_1 {
  height: 30px;
  width: 275px;
  text-align: center;
  background-color: #21b3b9;
  line-height: 30px;
}

.demo-image__preview1 {
  width: 250px;
  height: 290px;
  margin: 20px 60px;
  float: left;
}

.demo-image__preview2 {
  width: 250px;
  height: 290px;

  margin: 20px 460px;
  /* background-color: green; */
}

.error {
  margin: 100px auto;
  width: 50%;
  padding: 10px;
  text-align: center;
}

div {
  display: block;
}

.download_bt {
  padding: 10px 16px !important;
}

#Content {
  width: 85%;
  height: 800px;
  background-color: #ffffff;
  margin: 15px auto;
  display: flex;
  min-width: 1200px;
}

#info_patient {
  margin-top: 10px;
  margin-right: 160px;
}
</style>


