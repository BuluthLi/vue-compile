          <!-- v-if和v-show的区别 -->

<template>
  <div class="main">
    <div class="header">
      <img src="./topbg.jpg" alt>
      <!-- <img src="../../assets/list01.png" alt>
      <img src="./list02.png" alt> -->
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
          <!-- 动态的图片需要require或者import -->
          <!-- <img :src="goodsList[0].cover" alt class="item-bg"> 会被解析成src:./list01.png;打包后得不到数据，没有该路径-->
        </div>
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
export default {
  name: "Main",
  data() {
    return {
      rankList: [],
      goodsList: [
        { title: "智能语音主机", cover: list01, id: 1 },
        { title: "人体红外感应", cover: list02, id: 2 },
        { title: "智能开关", cover: list03, id: 3 },
        { title: "物联网门锁", cover: list04, id: 4 },
        { title: "智能摄像头", cover: list05, id: 5 },
        { title: "智能插座", cover: list06, id: 6 },
        { title: "智能环境监测", cover: list07, id: 7 }
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
    }
  },
  created() {
    console.log(
      `${process.env.API_HOST}index.php?s=/Campain/Questionnaire/GetRanking`
    );
    this.$http
      .get(
        process.env.API_HOST + "index.php?s=/Campain/Questionnaire/GetRanking",
        {}
      )
      .then(res => {
        console.log(res);
        res.data.every((item, index) => {
          // item.cover = "./list0" + item.goods_id + ".png";
          item.cover = this.goodsList[item.goods_id-1].cover;
          return true;
        });
        console.log(res.data);
        this.rankList = res.data;
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
        }
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
