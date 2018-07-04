<template>
  <div class="comm_container">
    <div class="top_tit">
      <p class="cal_top">{{curMonth}}月
        <span>本月已点亮{{staredList.length}}天</span>
      </p>
      <div class="calendar">
        <ul>
          <li v-for="item in curMonthTotalDays" :key="item" :class="[activeCircle==item-1?'on':'',staredList.includes(item)?'star':'']" @click="triggerDate(item)">
            <span v-if="item==curDate">
              今
            </span>
            <span v-else-if="item>curDate" style="color:#666666">
              {{item}}
            </span>
            <span v-else>
              {{item}}
            </span>
          </li>
        </ul>
      </div>
    </div>
    <div class="show_box">
      <!-- <swiper class="biaoqian" :list="demo01_list" v-model="demo01_index" @on-index-change="demo01_onIndexChange" :show-dots="false" :aspect-ratio="1.17333" :show-desc-mask="false"></swiper> -->
      <swiper class="biaoqian" v-model="demo01_index" @on-index-change="demo01_onIndexChange" :show-dots="false" :aspect-ratio="1.17333" :show-desc-mask="false">
        <swiper-item class="white tagItem" v-for="(item) in demo01_list" :key="item.id">
          <h2 class="cur_title">{{item.title}}</h2>
          <div class="tag_pic_box">
            <img :src="item.img" :alt="item.title" class="tagPicItem">
          </div>
          <div class="tag_bottom">
            <p>1111111111111</p>
            <p>--帅</p>
            <p>《一个人的战斗》</p>
          </div>
        </swiper-item>
      </swiper>
      <x-button @click.native="demo01_index--" :class="['biaoqian_btn_left',demo01_index==0?'disabled':'']" :disabled="demo01_index==0"></x-button>
      <x-button @click.native="demo01_index++" :class="['biaoqian_btn_right',demo01_index==curDate-1?'disabled':'']" :disabled="demo01_index==curDate-1"></x-button>
    </div>
    <button @click="saveCurPic">
      <span class="biaoqian_btn_download"></span>
      保存图片
      </button>
  </div>
</template>

<script>
import img from "../../assets/imgs/biaoqian.png";
export default {
  components: {},
  name: "login",
  data() {
    return {
      // swiper配置
      demo01_index: 0,
      demo01_list: [],
      // 当前选中项
      activeCircle: 0,
      // 点亮
      staredList: [2, 4]
    };
  },
  computed: {
    curMonth() {
      return new Date().getMonth() + 1;
    },
    curDate() {
      return new Date().getDate();
    },
    curMonthTotalDays() {
      return this.mGetDate();
    }
  },
  created: function() {
    // 设置默认值
    this.demo01_index = this.curDate - 1;
    this.activeCircle = this.curDate - 1;
  },
  mounted: function() {
    // 获取标签
    this.getTags()
  },
  methods: {
    getTags(){
      let curMonth = this.curMonth;
      let biaoqian_list = [];
      for (let i = 0; i < this.curDate; i++) {
        let curDate = this.prefixInteger(i+1, 2)
        biaoqian_list.push({
          id: i,
          url: "javascript:",
          img: img,
          title: `${curDate}/${curMonth}`
        });
      }
      this.demo01_list = biaoqian_list;
    },
    demo01_onIndexChange(index) {
      this.demo01_index = index;
      this.activeCircle = index;
    },
    mGetDate() {
      var date = new Date();
      var year = date.getFullYear();
      var month = date.getMonth() + 1;
      var d = new Date(year, month, 0);
      return d.getDate();
    },
    triggerDate(item) {
      if (item <= this.curDate) {
        this.demo01_index = item - 1;
      }
      this.activeCircle = item;
    },
    // 自动补0
    prefixInteger(num, n) {
        return (Array(n).join(0) + num).slice(-n);
    },
    // 保存当前图片
    saveCurPic() {
      var pic_index = this.demo01_index;
      var self = this
      this.$nextTick(()=>{
        var src = document.querySelectorAll('.tagPicItem')[pic_index].src;
        self.saveAs(img)
      })
    },
    saveAs(Url) {
      var blob = new Blob([""], { type: "application/octet-stream" });
      var url = URL.createObjectURL(blob);
      var a = document.createElement("a");
      a.href = Url;
      a.download = Url.replace(/(.*\/)*([^.]+.*)/gi, "$2").split("?")[0];
      var e = document.createEvent("MouseEvents");
      e.initMouseEvent("click",true,false,window,0,0,0,0,0,false,false,false,false,0,null);
      a.dispatchEvent(e);
      URL.revokeObjectURL(url);
    }
  }
};
</script>

<style lang="less" scoped>
.comm_container {
  width: 100%;
  background-color: #27292b;
  .cal_top {
    padding-left: 50px;
    line-height: 56px;
    height: 56px;
    span {
      padding-right: 30px;
      float: right;
      color: #c19b5c;
      font-size: 24px;
    }
  }
  .top_tit {
    padding-top: 32px;
    color: #fff;
    font-size: 30px;
    height: 200px;
    box-sizing: border-box;
  }
  .calendar {
    padding-top: 12px;
    height: 88px;
    ul {
      overflow-x: scroll;
      &::-webkit-scrollbar {
        display: none;
      }
      white-space: nowrap;
    }
    li {
      position: relative;
      text-align: center;
      list-style: none;
      display: inline-block;
      width: 82px;
      span {
        width: 64px;
        height: 64px;
        line-height: 64px;
      }
      &.on {
        span {
          display: block;
          border-radius: 50%;
          border: 2px solid #c19b5c;
        }
      }
      &.star {
        span {
          &:after {
            content: "";
            position: absolute;
            width: 4px;
            height: 4px;
            border-radius: 50%;
            bottom: 0;
            left: 50%;
            margin-left: -2px;
            background-color: #c19b5c;
          }
        }
      }
    }
  }
  .show_box {
    position: relative;
    background: #fff;
  }
  .biaoqian {
    .tagItem{
      width:750px;
      height:880px;
      .cur_title{
        color:#fff;
      }
      .tag_pic_box{

      }
      .tag_bottom{

      }
    }
  }
  .biaoqian_btn_left {
    width: 67px;
    height: 116px;
    position: absolute;
    top: 50%;
    margin-top: -58px;
    left: 6px;
    background: url("../../assets/imgs/spr_pic.png") no-repeat;
    background-position: -68px 0px;
    background-size: 167px 250px;
    &:after{
      border:none;
    }
    &.disabled {
      background-position: -68px -120px;
    }
  }
  .biaoqian_btn_right {
    width: 67px;
    height: 116px;
    position: absolute;
    top: 50%;
    margin-top: -58px;
    right: 6px;
    background: url("../../assets/imgs/spr_pic.png") no-repeat;
    background-position: 0 0;
    background-size: 167px 250px;
    &:after{
      border:none;
    }
    &.disabled {
      background-position: 0 -120px;
    }
  }
  .biaoqian_btn_download{
    width: 28px;
    height: 28px;
    background: url("../../assets/imgs/spr_pic.png") no-repeat;
    background-position: -139px 0;
    background-size: 167px 250px;
  }
}
</style>
