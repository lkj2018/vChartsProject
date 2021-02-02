<template>
  <div class="vChartsContainer">
    <el-container>
      <el-header height="64px">HEADER</el-header>
      <el-container>
        <el-aside width="220px">
          <div class="menuSide">
            <el-menu default-active="0" @select="selectMenu">
              <el-menu-item index="0">vCharts</el-menu-item>
              <el-menu-item index="1">Other</el-menu-item>
            </el-menu>
          </div>
        </el-aside>
        <el-main>
          <ul class="vCharts-ul" v-if="vChartsIsShow">
            <li class="vCharts-li" v-for="item in items" :key="item.id">
              <ve-histogram
                v-if="item.name == 'histogram'"
                :data="chartData"
              ></ve-histogram>
              <ve-line v-if="item.name == 'line'" :data="chartData"></ve-line>
              <ve-pie v-if="item.name == 'pie'" :data="chartData"></ve-pie>
              <ve-ring v-if="item.name == 'ring'" :data="chartData"></ve-ring>
              <ve-map v-if="item.name == 'map'" :data="chartMapData"></ve-map>
              <div id="map"></div>
            </li>
          </ul>
          <div class="swiper-box" v-if="otherIsShow">
            <div class="swiper-container">
              <div class="swiper-wrapper">
                <div class="swiper-slide swiper-png1">1</div>
                <div class="swiper-slide swiper-png2">2</div>
                <div class="swiper-slide swiper-png3">3</div>
                <div class="swiper-slide swiper-png4">4</div>
                <div class="swiper-slide swiper-png5">5</div>
              </div>
            </div>
            <div>
              <div class="cropper-app">
                <el-form
                  :model="formValidate"
                  :rules="ruleValidate"
                  ref="formValidate"
                  label-width="100px"
                  class="demo-ruleForm"
                >
                  <el-form-item label="封面上传" prop="mainImage">
                    <div class="list-img-box">
                      <div v-if="formValidate.mainImage !== ''">
                        <img
                          :src="formValidate.mainImage"
                          style="width: 300px; height: 150px"
                          alt="自定义封面"
                        />
                      </div>
                      <div
                        v-else
                        class="upload-btn"
                        style="height: 120px"
                        @click="uploadPicture('flagImg')"
                      >
                        <i class="el-icon-plus" style="font-size: 30px"></i>
                        <span>封面设置</span>
                      </div>
                    </div>
                    <input
                      type="hidden"
                      v-model="formValidate.mainImage"
                      placeholder="请添加封面"
                    />
                  </el-form-item>
                </el-form>
                <!-- 剪裁组件弹窗 -->
                <el-dialog
                  title="裁切封面"
                  :visible.sync="cropperModel"
                  width="950px"
                  center
                >
                  <cropper-image
                    :Name="cropperName"
                    @uploadImgSuccess="handleUploadSuccess"
                    ref="child"
                  >
                  </cropper-image>
                </el-dialog>
                <!--查看大封面-->
                <el-dialog title="查看大封面" :visible.sync="imgVisible" center>
                  <img
                    :src="imgName"
                    v-if="imgVisible"
                    style="width: 100%"
                    alt="查看"
                  />
                </el-dialog>
              </div>
            </div>
          </div>
        </el-main>
      </el-container>
      <el-footer height="50px">FOOTER</el-footer>
    </el-container>
  </div>
</template>
<script>
import Swiper from "swiper";
import CropperImage from "@/components/CropperImage";
export default {
  name: "",
  mixins: "",
  components: { CropperImage },
  props: {},
  data() {
    return {
      vChartsIsShow: true,
      otherIsShow: false,
      items: [
        {
          id: 0,
          name: "histogram",
        },
        {
          id: 1,
          name: "line",
        },
        {
          id: 2,
          name: "pie",
        },
        {
          id: 3,
          name: "ring",
        },
        {
          id: 4,
          name: "map",
        },
      ],
      chartData: {
        columns: ["日期", "访问用户", "下单用户", "下单率"],
        rows: [
          { 日期: "1/1", 访问用户: 1393, 下单用户: 1093, 下单率: 0.32 },
          { 日期: "1/2", 访问用户: 3530, 下单用户: 3230, 下单率: 0.26 },
          { 日期: "1/3", 访问用户: 2923, 下单用户: 2623, 下单率: 0.76 },
          { 日期: "1/4", 访问用户: 1723, 下单用户: 1423, 下单率: 0.49 },
          { 日期: "1/5", 访问用户: 3792, 下单用户: 3492, 下单率: 0.323 },
          { 日期: "1/6", 访问用户: 4593, 下单用户: 4293, 下单率: 0.78 },
        ],
      },
      chartMapData: {
        columns: ["位置", "税收", "人口", "面积"],
        rows: [
          { 位置: "吉林", 税收: 123, 人口: 123, 面积: 92134 },
          { 位置: "北京", 税收: 1223, 人口: 2123, 面积: 29234 },
          { 位置: "上海", 税收: 2123, 人口: 1243, 面积: 94234 },
          { 位置: "浙江", 税收: 4123, 人口: 5123, 面积: 29234 },
        ],
      },
      formValidate: {
        mainImage: "",
      },
      ruleValidate: {
        mainImage: [{ required: true, message: "请上传封面", trigger: "blur" }],
      },
      //裁切图片参数
      cropperModel: false,
      cropperName: "",
      imgName: "",
      imgVisible: false,
    };
  },
  computed: {},
  watch: {},
  created() {},
  mounted() {
    this.$nextTick(() => {
      this.initSwiper();
    });
  },
  destroyed() {},
  methods: {
    selectMenu(index, indexPath) {
      switch (index) {
        case "0":
          this.vChartsIsShow = true;
          this.otherIsShow = false;
          break;
        case "1":
          this.vChartsIsShow = false;
          this.otherIsShow = true;
          break;
      }
    },
    initSwiper() {
      let mySwiper = new Swiper(".swiper-container", {
        // observer: true,
        initialSlide: 2,
        // autoplay: true,
      });
    },
    //封面设置
    uploadPicture(name) {
      this.cropperName = name;
      this.cropperModel = true;
    },
    //图片上传成功后
    handleUploadSuccess(data) {
      console.log(data);
      switch (data.name) {
        case "flagImg":
          this.formValidate.mainImage = "http://ydfblog.cn/dfs/" + data.url;
          console.log("最终输出" + data.name);
          break;
      }
      this.cropperModel = false;
    },
  },
};
</script>
<style lang="less">
.vChartsContainer {
  width: 100%;
  height: 100%;
  .el-container {
    width: 100%;
    height: 100%;
    .el-header {
      background: #1890ff;
      padding: 0;
    }
    .el-container {
      .el-main {
        padding: 0;
        ul {
          width: 100%;
          .vCharts-li {
            width: 30%;
            margin-right: 3%;
            float: left;
          }
        }
        .swiper-box {
          width: 500px;
          height: 250px;
          padding: 30px;
          .swiper-container {
            width: 100%;
            height: 100%;
            .swiper-wrapper {
              .swiper-png1 {
                background: url("../../assets/1.png");
              }
              .swiper-png2 {
                background: url("../../assets/2.png");
              }
              .swiper-png3 {
                background: url("../../assets/3.png");
              }
              .swiper-png4 {
                background: url("../../assets/4.png");
              }
              .swiper-png5 {
                background: url("../../assets/5.png");
              }
            }
          }
          .upload-list-cover {
            position: absolute;
            top: 0;
            bottom: 0;
            left: 0;
            right: 0;
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
            padding: 0 40px;
            align-items: center;
            background: rgba(0, 0, 0, 0.6);
            opacity: 0;
            transition: opacity 1s;
          }
          .cover_icon {
            font-size: 30px;
          }
          .upload-btn {
            display: -webkit-box;
            display: -ms-flexbox;
            display: flex;
            -ms-flex-wrap: wrap;
            flex-wrap: wrap;
            -webkit-box-pack: center;
            -ms-flex-pack: center;
            justify-content: center;
            -webkit-box-align: center;
            -ms-flex-align: center;
            align-items: center;
            border: 1px solid #cccccc;
            border-radius: 5px;
            overflow: hidden;
            box-shadow: 0 0 1px #cccccc;
          }
          .upload-btn:hover {
            border: 1px solid #69b7ed;
          }
          .upload-btn i {
            margin: 5px;
          }
        }
      }
    }
    .el-footer {
      background: #1890ff;
      padding: 0;
    }
  }
}
</style>