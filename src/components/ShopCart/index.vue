<template>
  <div class="cart">
    <h4>全部商品</h4>
    <!-- 商品列表 -->
    <div class="cart-main">
      <div class="cart-th">
        <div class="cart-th1">全部</div>
        <div class="cart-th2">商品</div>
        <div class="cart-th3">单价（元）</div>
        <div class="cart-th4">数量</div>
        <div class="cart-th5">小计（元）</div>
        <div class="cart-th6">操作</div>
      </div>
      <div
        class="cart-body"
        v-for="(shopCart, index) in shopCartList"
        :key="index"
      >
        <ul class="cart-list">
          <!-- 选中 -->
          <li class="cart-list-con1">
            <input
              type="checkbox"
              :checked="shopCart.isChecked"
              name="chk_list"
              @click="updateIsChecked(shopCart, index)"
            />
          </li>

          <!-- 图片以及描述 -->
          <li class="cart-list-con2">
            <img :src="shopCart.imgUrl" />
            <div class="item-msg">{{ shopCart.describe }}</div>
          </li>

          <!-- 单价 -->
          <li class="cart-list-con4">
            <span class="price">{{ shopCart.price }}</span>
          </li>

          <!-- 数量操作 -->
          <li class="cart-list-con5">
            <a
              href="javascript:void(0)"
              class="mins"
              @click="changeNum(-1, shopCart, index)"
              >-</a
            >
            <input
              autocomplete="off"
              type="text"
              minnum="1"
              class="itxt"
              :value="shopCart.num"
              @change="changeNum(0, shopCart, index, $event.target.value * 1)"
            />
            <a
              href="javascript:void(0)"
              class="plus"
              @click="changeNum(1, shopCart, index)"
              >+</a
            >
          </li>

          <!-- 小计 -->
          <li class="cart-list-con6">
            <span class="sum">{{ shopCart.num * shopCart.price }}</span>
          </li>

          <!-- 操作 -->
          <li class="cart-list-con7">
            <a href="javascript:;" class="sindelet" @click="deleteCart(index)"
              >删除</a
            >
            <br />
            <a href="#none">移到收藏</a>
          </li>
        </ul>
      </div>
    </div>

    <!-- 底部总体操作部分 -->
    <div class="cart-tool">
      <div class="select-all">
        <input
          class="chooseAll"
          type="checkbox"
          v-model="isCheckAll"
          @click="updateAllChecked()"
        />
        <span style="margin-left: 5px">全选</span>
      </div>
      <div class="option">
        <!-- <a href="javascript:;" @click="deleteCartAll">删除选中的商品</a> -->
        <a href="#none">移到我的关注</a>
        <a href="#none">清除下柜商品</a>
      </div>
      <div class="money-box">
        <div class="chosed">
          已选择 <span>{{ arrLength }}</span
          >件商品
        </div>
        <div class="sumprice">
          <em>总价（不含运费） ：</em>
          <i class="summoney">{{ allMoney }}</i>
        </div>
        <div class="sumbtn">
          <a class="sum-btn" href="javascript:;">结算</a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ShopCart",
  data() {
    return {
      shopCartList: [
        {
          imgUrl: require("./images/goods1.png"),
          describe:
            "新款产品,新款电视机,1000英寸,256GB大存储,明月灰,智能联网电视",
          price: 1250,
          num: 3,
          total: 0,
          isChecked: false,
        },
        {
          imgUrl: require("./images/goods2.png"),
          describe:
            "新款产品,新款电视机,1000英寸,256GB大存储,明月灰,智能联网电视",
          price: 1250,
          num: 3,
          total: 0,
          isChecked: false,
        },
        {
          imgUrl: require("./images/goods3.png"),
          describe:
            "新款产品,新款电视机,1000英寸,256GB大存储,明月灰,智能联网电视",
          price: 1250,
          num: 3,
          total: 0,
          isChecked: false,
        },
      ],
      isCheckAll: false,
      arrLength: 0,
    };
  },
  methods: {
    // 修改购物车的数量
    changeNum(disNum, shopCart, index, value) {
      if (disNum == -1) {
        // 点击减号
        if (shopCart.num > 1) {
          this.shopCartList[index].num = this.shopCartList[index].num - 1;
        } else {
          console.log(shopCart.num, "不重要");
        }
      } else if (disNum == 1) {
        this.shopCartList[index].num = this.shopCartList[index].num + 1;
      } else if (disNum == 0) {
        let newValue = Math.round(value);
        if (newValue >= 1) {
          this.shopCartList[index].num = newValue;
        } else {
          // 当输入不合法的数字时 直接修正为1,因为当前不是接口请求回来的数据 所以简单操作了
          alert("修改数量失败");
          console.log(this.shopCartList[index].num);
          this.shopCartList[index].num = 1;
        }
      }
    },
    // 删除单个购物车
    deleteCart(index) {
      this.shopCartList.splice(index, 1);
      alert("删除购物车成功");
    },
    // 点击修改单个购物车状态
    updateIsChecked(shopCart, index) {
      this.shopCartList[index].isChecked = !shopCart.isChecked;
      console.log(this.shopCartList[index].isChecked, "当前选中状态");
      let arr = [];
      this.shopCartList.map((item, index) => {
        console.log(item.isChecked, index);
        if (item.isChecked) {
          arr.push(index);
        }
      });
      this.arrLength = arr.length;
      if (arr.length == this.shopCartList.length) {
        this.isCheckAll = true;
      } else {
        this.isCheckAll = false;
      }
    },
    // 全选修改
    updateAllChecked() {
      this.isCheckAll = !this.isCheckAll;
      if (!this.isCheckAll) {
        this.shopCartList.map((item) => {
          item.isChecked = false;
        });
        this.arrLength = 0;
      } else {
        this.shopCartList.map((item) => {
          item.isChecked = true;
        });
        this.arrLength = this.shopCartList.length;
      }
    },
    // 删除所有已经选中的
    deleteCartAll() {
      // 准备参数
      this.shopCartList.map((item, index) => {
        if (item.isChecked === true) {
          console.log(item, index);
        }
      });
      alert("删除成功");
    },
  },
  computed: {
    allMoney() {
      return this.shopCartList.reduce((prev, item) => {
        if (item.isChecked) {
          prev += item.num * item.price;
        }
        return prev;
      }, 0);
    },
  },
};
</script>

<style lang="less" scoped>
.cart {
  width: 1200px;
  margin: 0 auto;

  h4 {
    margin: 9px 0;
    font-size: 14px;
    line-height: 21px;
  }

  .cart-main {
    .cart-th {
      background: #f5f5f5;
      border: 1px solid #ddd;
      padding: 10px;
      overflow: hidden;

      & > div {
        float: left;
      }

      .cart-th1 {
        width: 15%;

        input {
          vertical-align: middle;
        }

        span {
          vertical-align: middle;
        }
      }

      .cart-th2 {
        width: 25%;
      }

      .cart-th3,
      .cart-th4,
      .cart-th5,
      .cart-th6 {
        width: 15%;
      }
    }

    .cart-body {
      margin: 15px 0;
      border: 1px solid #ddd;

      .cart-list {
        padding: 10px;
        border-bottom: 1px solid #ddd;
        overflow: hidden;

        & > li {
          float: left;
        }

        .cart-list-con1 {
          width: 15%;
          margin-top: 30px;
        }

        .cart-list-con2 {
          width: 25%;

          img {
            width: 82px;
            height: 82px;
            float: left;
            margin-left: 30px;
          }

          .item-msg {
            float: left;
            width: 150px;
            margin: 10px 5px 0;
            line-height: 18px;
          }
        }

        .cart-list-con4 {
          width: 13%;
          margin: 25px 0 0 10px;
        }

        .cart-list-con5 {
          width: 13%;
          margin: 10px 0 0 55px;

          .mins {
            border: 1px solid #ddd;
            border-right: 0;
            float: left;
            color: #666;
            width: 6px;
            text-align: center;
            padding: 8px;
          }

          input {
            border: 1px solid #ddd;
            width: 40px;
            height: 33px;
            float: left;
            text-align: center;
            font-size: 14px;
          }

          .plus {
            border: 1px solid #ddd;
            border-left: 0;
            float: left;
            color: #666;
            width: 6px;
            text-align: center;
            padding: 8px;
          }
        }

        .cart-list-con6 {
          width: 10%;
          margin-top: 20px;

          .sum {
            font-size: 16px;
          }
        }

        .cart-list-con7 {
          width: 17%;
          margin: 15px 0 0 10px;
          transform: translateX(18px);

          a {
            color: #666;
          }
        }
      }
    }
  }

  .cart-tool {
    overflow: hidden;
    border: 1px solid #ddd;

    .select-all {
      padding: 10px;
      overflow: hidden;
      float: left;
      margin-top: 10px;

      span {
        vertical-align: middle;
      }

      input {
        vertical-align: middle;
      }
    }

    .option {
      padding: 10px;
      overflow: hidden;
      float: left;
      margin-top: 10px;

      a {
        float: left;
        padding: 0 10px;
        color: #666;
      }
    }

    .money-box {
      float: right;

      .chosed {
        line-height: 26px;
        float: left;
        padding: 0 10px;
        margin-top: 10px;
      }

      .sumprice {
        width: 200px;
        line-height: 22px;
        float: left;
        padding: 0 10px;
        margin-top: 10px;

        .summoney {
          color: #c81623;
          font-size: 16px;
          margin-top: 15px;
        }
      }

      .sumbtn {
        float: right;

        a {
          display: block;
          position: relative;
          width: 96px;
          height: 52px;
          line-height: 52px;
          color: #fff;
          text-align: center;
          font-size: 18px;
          font-family: "Microsoft YaHei";
          background: #e1251b;
          overflow: hidden;
        }
      }
    }
  }
}
</style>