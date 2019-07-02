          <!-- v-if和v-show的区别 -->

<template>
  <div class="main">
    <div class="header">
      <img src="./topbg.jpg" alt>
      <!-- <img src="../../assets/list01.png" alt>
      <img src="./list02.png" alt>-->
    </div>
    <div class="content">
      <div class="rank">
        <img src="./listbg.png" alt class="rank-img">
        <div
          class="rank-item"
          v-for="(item,index) in rankList"
          :key="index"
          :class="rankItem(index)"
        >
          <div class="rank-rate" v-show="index>=3">{{item.rank}}</div>
          <img class="rank-cover" :src="item.cover" alt>
          <div class="rank-name">{{item.name}}</div>
          <div class="rank-count">
            <span>{{item.number}}</span>次
          </div>
        </div>
      </div>
      <img class="titleone" src="./title01.png" alt>
      <div class="goods-box">
        <div class="good-item" v-for="(item,index) in goodsList" :key="index">
          <img src="./details_small.png" alt class="item-bg">
          <img :src="calPickUrl(item.selected)" alt class="item-pick" @click="onSendPick(item.id)">
          <div class="item-title">{{item.title}}</div>
          <img :src="item.goodsCover" :width="item.width" alt class="item-cover">
          <div class="icon-tips" @click="onShowTips(item)"></div>
          <!-- 动态的图片需要require或者import -->
          <!-- <img :src="goodsList[0].cover" alt class="item-bg"> 会被解析成src:./list01.png;打包后得不到数据，没有该路径-->
        </div>
      </div>
    </div>
    <div class="alert-wrapper" v-show="showTipsFlag">
      <div class="shade"></div>
      <img src="./details-bg.png" class="bg-img" alt>
      <div class="alert-box">
        <img src="./X.png" alt width="12" height="12" class="alert-close" @click="onHideTipsFlag">
        <div class="tips">
          <div class="tips-title">{{showItem.title}}</div>
          <img class="tips-cover" :src="showItem.goodsCover">
          <div class="tips-text">{{showItem.tips}}</div>
        </div>
        <img alt class="img-left" :src="showItem.go">
        <img
          :src="calPickUrl(showItem.selected)"
          alt
          class="img-right"
          @click="onSendPick(showItem.id)"
        >
      </div>
    </div>
  </div>
</template>

<script>
import list01 from "./list01.png";
import list02 from "./list02.png";
import list03 from "./list03.png";
import list04 from "./list04.png";
import list05 from "./list05.png";
import list06 from "./list06.png";
import list07 from "./list07.png";
import smart01 from "./smart01.png";
import smart02 from "./smart02.png";
import smart03 from "./smart03.png";
import smart04 from "./smart04.png";
import smart05 from "./smart05.png";
import smart06 from "./smart06.png";
import smart07 from "./smart07.png";
import picknormal from "./PICK-normal.png";
import pickselected from "./PICK-click.png";
import go from "./Go.png";
// import detailBg from "./details-bg.png";
// var user_id = 2401452;
var user_id = 138701;
var send = false;
export default {
  name: "Main",
  data() {
    return {
      rankList: [],
      count: 3,
      showTipsFlag: false,
      // 默认是第一个
      showItem: {
        title: "智能语音主机",
        // rankCover: list01,
        // goodsCover: smart01,
        rankCover:
          "https://wsaiosscdn.yfway.com/ds/Uploads/Picture/2019/04/11/5caeeea872f1d.png",
        goodsCover:
          "https://wsaiosscdn.yfway.com/ds/Uploads/Picture/2019/04/26/5cc2b51ce172e.png",
        width: 58,
        id: 1,
        selected: false,
        tips:
          "基于人工智能的智能管家机器人可连接云端服务器，运用大数据精准处理语音信息，单独、组合或按照场景控制家电，设置个性场景，人性化的语音交交互，可实现各种功能，是智能家居的“大脑”。",
        go: go
      },
      // 固定展示数组
      goodsList: [
        {
          title: "智能语音主机",
          rankCover: list01,
          goodsCover: smart01,
          width: 58,
          id: 1,
          selected: false,
          tips:
            "基于人工智能的智能管家机器人可连接云端服务器，运用大数据精准处理语音信息，单独、组合或按照场景控制家电，设置个性场景，人性化的语音交交互，可实现各种功能，是智能家居的“大脑”。"
        },
        {
          title: "人体红外感应",
          rankCover: list02,
          goodsCover: smart02,
          width: 41,
          id: 2,
          selected: false,
          tips:
            "通过感应人体温度，判断家里人或宠物移动，装在门口，没人在家也能掌握家庭安全情况，搭配其他智能家居产品，可体验更多智能新玩法。"
        },
        {
          title: "智能开关",
          rankCover: list03,
          goodsCover: smart03,
          width: 58,
          id: 3,
          selected: false,
          tips:
            "智能开关颠覆了传统开关的操控方式，可一键管理室内灯光，并可实现远程控制和场景联动。"
        },
        {
          title: "物联网门锁",
          rankCover: list04,
          goodsCover: smart04,
          width: 66,
          id: 4,
          selected: false,
          tips:
            "适用于别墅、公寓、小区、大厦等多种场所，有多种开锁方式，如指纹、密码、APP、微信、磁卡等。可根据不同指纹或不同开锁方式，绑定不同的情景模式。"
        },
        {
          title: "智能摄像头",
          rankCover: list05,
          goodsCover: smart05,
          width: 46,
          id: 5,
          selected: false,
          tips:
            "24小时智能监控门外，自动智能侦测拍照、录像，可有效震慑门前踩点行为，日夜两用可清晰记录访客信息，并有红外功能，晚上也能看清漆黑楼道，支持同账号多用户登录，可进行视频通话，访客照片自动发送到主人手机。"
        },
        {
          title: "智能插座",
          rankCover: list06,
          goodsCover: smart06,
          width: 55,
          id: 6,
          selected: false,
          tips:
            "支持手机远程控制和手机控制，兼容各种电器设备，定时启动及联动智能家居场景，配置简单方便，在家庭及办公场所等需要远程控制的电器中广泛应用。"
        },
        {
          title: "智能环境监测",
          rankCover: list07,
          goodsCover: smart07,
          width: 60,
          id: 7,
          selected: false,
          tips:
            "室内二氧化碳传感器、PM2.5监测、有害气体监测，温湿度监测等多种传感器，可以实时监控环境质量 ，调动家里的空调、空气净化器等设备，调节家中的环境质量。"
        }
      ]
    };
  },
  computed: {},
  methods: {
    rankItem(index) {
      if (index >= 3) {
        return "rank-item-other";
      }
      return "rank-item" + index;
    },
    calPickUrl(selected) {
      return selected ? pickselected : picknormal;
    },
    onSendPick(id) {
      if (this.count <= 0 || this.goodsList[id - 1].selected || send)
        return false;
      // 提交用户的选择
      console.log(process.env.API_HOST);
      send = true;
      let params = new FormData();
      params.append("user_id", user_id);
      params.append("goods_id", id);
      this.$http
        .post(
          process.env.API_HOST +
            "index.php?s=/Campain/Questionnaire/LogUserSelect",
          params
        )
        .then(res => {
          console.log(res.data);
          if (res.data.status == 1) {
            console.log(res.data.message);
            send = false;
            this.count--;
            this.goodsList[id - 1].selected = true;
            let item = this.goodsList[id - 1];
            if (this.showTipsFlag) {
              this.showItem = { ...this.showItem, ...item };
            }
          }
        })
        .catch(err => {
          console.log(err);
        });
    },
    onShowTips(item) {
      this.showItem = { ...this.showItem, ...item };
      console.log(this.showItem);
      this.showTipsFlag = true;
    },
    onHideTipsFlag() {
      this.showTipsFlag = false;
    }
  },
  created() {
    let params = new FormData();
    params.append("user_id", user_id);
    // 请求排行榜数据
    this.$http
      .get(
        process.env.API_HOST + "index.php?s=/Campain/Questionnaire/GetRanking",
        {}
      )
      .then(res => {
        console.log(res.data);
        res.data.every((item, index) => {
          // item.cover = "./list0" + item.goods_id + ".png";
          item.cover = this.goodsList[item.goods_id - 1].rankCover;
          return true;
        });
        // console.log(res.data);
        this.rankList = res.data;
      })
      .catch(err => {
        console.log(err);
      });
    // 请求当前用户的选择数据
    this.$http
      .post(
        process.env.API_HOST +
          "index.php?s=/Campain/Questionnaire/GetUserSelect",
        params
      )
      .then(res => {
        this.count = res.data.remind_number;
        if (res.data.remind_number < 3) {
          res.data.user_select.map((item, index) => {
            item = ~~item;
            this.goodsList[item - 1].selected = true;
          });
        }
      })
      .catch(err => {
        console.log(err);
      });
  },
  mounted() {}
};
</script>

<style scoped lang="less">
.main {
  background: #010a29;
  position: relative;
  .header {
    width: 100%;
    height: auto;
    img {
      width: 100%;
    }
  }
  .content {
    background: url("./repeat.jpg") repeat-y;
    background-size: 100% auto;
    padding-bottom: 108px;
    .rank {
      position: relative;
      .rank-img {
        display: block;
        width: 94%;
        margin: 0 auto;
      }
      .rank-item {
        position: absolute;
        font-size: 14px;
        color: white;
        font-family: "PingFang-SC-Medium";
        text-align: center;
        .rank-cover {
          width: 100%;
        }
        .rank-name {
          margin-top: 18%;
          width: 100%;
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
        }
        .rank-count {
          margin-top: 4%;
          span {
            font-size: 16px;
            font-family: " PingFang-SC-Bold";
          }
        }
        &.rank-item0 {
          top: -13.5%;
          left: 50%;
          width: 30%;
          transform: translateX(-50%);
        }
        &.rank-item1 {
          top: -3.5%;
          left: 9%;
          width: 23%;
        }
        &.rank-item2 {
          top: -1.5%;
          right: 9%;
          width: 23%;
          .rank-name {
            margin-top: 16%;
          }
        }
        // 其他排名
        &.rank-item-other {
          display: flex;
          /* flex-wrap: wrap; */
          width: 100%;
          padding: 0 25px 0 20px;
          box-sizing: border-box;
          align-items: center;
          .rank-cover {
            width: 67px;
            // height: 100%;
            margin-left: 22px;
          }
          .rank-name {
            margin-top: 0%;
            margin-left: 22px;
            text-align: left;
          }
          .rank-count {
            margin-top: 0%;
          }
        }
        .rank-other(5, 36%);
        .rank-other(6, 52%);
        .rank-other(7, 70%);
        .rank-other(8, 87%);
      }
    }
    .titleone {
      width: 100%;
      margin-top: 50px;
    }
    .goods-box {
      width: 100%;
      .good-item {
        display: inline-block;
        width: 44%;
        margin: 30px 3% 0 3%;
        position: relative;
        &:first-child {
          margin-top: 0;
        }
        .item-bg {
          width: 100%;
          cursor: pointer;
        }
        .item-title {
          position: absolute;
          z-index: 30;
          top: 2%;
          left: 12%;
          color: #65c6e7;
          font-size: 12px;
          cursor: pointer;
        }
        .item-cover {
          position: absolute;
          // width: 52%;
          z-index: 30;
          top: 40%;
          left: 50%;
          transform: translate(-50%, -50%);
          cursor: pointer;
        }
        .item-pick {
          margin-top: 20px;
          margin-left: 21%;
          width: 58%;
          cursor: pointer;
        }
        .icon-tips {
          position: absolute;
          z-index: 32;
          top: 16%;
          right: 9%;
          width: 10%;
          background: transparent;
          border-radius: 50%;
          height: 7%;
          cursor: pointer;
        }
      }
    }
  }
  .alert-wrapper {
    position: fixed;
    z-index: 60;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    .shade {
      position: absolute;
      background: rgba(0, 0, 0, 0.9);
      z-index: 100;
      width: 100%;
      height: 100%;
      top: 0;
      left: 0;
    }
    .bg-img {
      width: 100%;
      position: absolute;
      z-index: 101;
      margin: 0 auto;
      display: block;
    }

    .alert-box {
      width: 293px;
      height: auto;
      position: absolute;
      z-index: 102;
      display: block;
      margin-top: 40%;
      left: 50%;
      transform: translateX(-147px);
      font-size: 12px;
      line-height: 15px;
      color: white;
      text-align: center;
      .alert-close {
        position: absolute;
        z-index: 103;
        right: 8px;
        top: -5px;
      }
      .tips {
        width: 293px;
        height: 182px;
        background: url("./details_big.png") no-repeat;
        background-size: contain;
        position: relative;
        display: flex;
        align-items: center;
        justify-content: center;
        padding-top: 10px;

        .tips-title {
          position: absolute;
          z-index: 110;
          top: 7px;
          left: 20px;
          color: rgb(101, 198, 231);
        }
        .tips-cover {
          width: 60px;
          margin: 0 20px;
        }
        .tips-text {
          width: 165px;
          height: auto;
          max-height: 105px;
          overflow: hidden;
          text-align: left;
        }
      }
      .img-left,
      .img-right {
        width: 96px;
        height: 28px;
        margin-top: 32px;
      }
      .img-left {
        margin-right: 35px;
      }
    }
  }
}
.rank-other(@index,@top) {
  &:nth-child(@{index}) {
    // top: @index*9%;
    top: @top;
  }
}
</style>
