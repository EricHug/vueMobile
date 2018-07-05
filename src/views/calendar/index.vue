<template>
  <div class="comm_container">
    <div class="top_tit">
      <p class="cal_top">{{curMonth}}月
        <span>本月已点亮{{staredList.length}}天</span>
      </p>
      <div class="calendar">
        <ul>
          <li v-for="item in curMonthTotalDays" :key="item" :class="[activeCircle==item?'on':'',staredList.includes(item)?'star':'']" @click="triggerDate(item)">
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
      <swiper class="biaoqian" v-model="demo01_index" @on-index-change="demo01_onIndexChange" :show-dots="false" :aspect-ratio="1.13333" :show-desc-mask="false">
        <swiper-item class="white tagItem" v-for="(item) in demo01_list" :key="item.id">
          <div class="sw-container">
            <h2 class="cur_title">{{item.title}}</h2>
            <p class="cur_title_cg">{{item.nl}}</p>
              <div class="tag_pic_box">
                <img :src="item.img" :alt="item.title" class="tagPicItem">
              </div>
              <div class="tag_bottom">
                <p class="tag_sentence">而是整个战斗，他一个人的力量背后，其实是广大人民群众的力量。其实是广大人民群众的而是整个众，人民群众的而是整个众。</p>
                <p class="tag_author">- 莱奥纳多琴，书友</p>
                <p class="tag_bk_name">《一个人的战斗》</p>
              </div>
              </div>
        </swiper-item>
      </swiper>
      <x-button @click.native="demo01_index--" :class="['biaoqian_btn_left',demo01_index==0?'disabled':'']" :disabled="demo01_index==0"></x-button>
      <x-button @click.native="demo01_index++" :class="['biaoqian_btn_right',demo01_index==curDate-1?'disabled':'']" :disabled="demo01_index==curDate-1"></x-button>
    </div>
    <x-button class="saveBtn" @click.native="saveCurPic">
      <span class="biaoqian_btn_download"></span>
      保存图片
    </x-button>
  </div>
</template>

<script>
import img from "../../assets/imgs/tag_1.png";
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
          nl: '戊戌年 戊午月 已末日',
          url: "javascript:",
          img: img,
          title: `${curDate}/${curMonth}`
        });
      }
      this.demo01_list = biaoqian_list;
    },
    demo01_onIndexChange(index) {
      this.demo01_index = index;
      this.activeCircle = index + 1;
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
      }else{
        return false;
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
  height: 100%;
  padding-bottom:20px;
  background: #f3f3f3;
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
    margin-bottom: 30px;
    color: #fff;
    font-size: 30px;
    height: 200px;
    box-sizing: border-box;
    background-color: #27292b;
    box-shadow: 0 5px 18px rgba(0,0,0,.1);
  }
  .calendar {
    margin-top:4px;
    padding-top: 8px;
    padding-bottom: 8px;
    height: 88px;
    ul {
      -webkit-overflow-scrolling: touch;
      padding:4px 0;
      overflow-y: hidden;
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
        margin: 0 auto;
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
            width: 8px;
            height: 8px;
            border-radius: 50%;
            bottom: -2px;
            left: 50%;
            margin-left: -4px;
            background-color: #c19b5c;
          }
        }
      }
    }
  }
  .show_box {
    position: relative;
  }
  .biaoqian {
    .tagItem{
      width:750px;
      height:820px;
      .sw-container{
        width:638px;
        height:820px;
        margin-top:30px;
        position:relative;
        margin:0 auto;
        box-shadow: 0 8px 30px rgba(0,0,0,.1);
        background: #fff;
        border-radius: 20px;
        overflow: hidden;
      }
      .cur_title{
        position:absolute;
        top:16px;
        color:#fff;
        font-size: 72px;
        padding-left: 50px;
        font-weight: normal;
        font-style: normal;
      }
      .cur_title_cg{
        position:absolute;
        top:116px;
        font-size: 20px;
        color:#fff;
        padding-left: 50px;
      }
      .tag_pic_box{
        width:100%;height:558px;
        margin:0 auto;
        overflow: hidden;
        img{
          width:100%;
          height:100%;
        }
      }
      .tag_bottom{
        padding:10px 34px;
        .tag_author,.tag_bk_name{
          font-size: 20px;
          color:#666;
          text-align: right;
        }
        .tag_sentence{
          padding:6px 0;
          color:#666;
          font-size: 28px;
          line-height: 48px;
        }
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
    display: inline-block;
    width: 28px;
    height: 28px;
    vertical-align: middle;
    background: url("../../assets/imgs/spr_pic.png") no-repeat;
    background-position: -139px 0;
    background-size: 167px 250px;
  }
  .saveBtn{
    position: relative;
    width:240px;height:80px;
    margin:0 auto;
    line-height: 80px;text-align: center;
    z-index: 11;
    background-color: #fff;
    box-shadow: 0 8px 30px rgba(0,0,0,.1);
    color:#393939;
    font-size: 28px;
    border-radius:40px;
  }
}
</style>
