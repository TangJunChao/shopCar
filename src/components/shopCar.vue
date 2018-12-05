<template>
  <div class="container">
    <div class="row">
      <div class="col-md-12">
        <h2 class="page-header">购物车</h2>
        <table class="table-bordered shopCarTable" width="100%">
          <thead>
            <tr>
              <th>
                <label><input type="checkbox" @click="check_all" :checked="check_goods.length === cart_list.length"> 全选</label>
              </th>
              <th>商品名称</th>
              <th>商品价格</th>
              <th>商品数量</th>
              <th>操作</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item , index) in cart_list" :key="index">
              <td><input :value="item" v-model="check_goods" type="checkbox"> </td>
              <td>{{ item.goods_name }}</td>
              <td>{{ item.goods_price }}</td>
              <td class="col-xs-3">
                <span class="btn btn-default" @click="reduce(item)">-</span>
                <input class="btn-group" type="number" v-model="item.num" name="" id="">
                <span class="btn btn-default" @click="add(item)">+</span>
              </td>
              <td>
                <button class="btn btn-danger btn-sm" @click="delete_num(item)">删除</button>
              </td>
            </tr>
            <tr>
             <td colspan="5" v-if="cart_list.length===0">暂无数据</td>
            </tr>
          </tbody>
        </table>
        <div class="total">
          总计：共{{ cart_list.length }}种商品，已选择{{ total_num }}件
          <div class="pull-right">
            合计：{{ total_price }}元
            <a href="#" class="btn btn-success" @click.prevent="goSubmit" :disabled="check_goods.length<=0">去结算</a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'shopCar',
  data(){
    return {
      cart_list:[
        {
          goods_name: 'iPhone X',
          goods_price: '8999',
          num: '2'
        },
        {
          goods_name: 'iPhone 8',
          goods_price: '4799',
          num: '10'
        },
        {
          goods_name: '华为 P10',
          goods_price: '6999',
          num: '5'
        },
        {
          goods_name: '小米8',
          goods_price: '3999',
          num: '30'
        }
      ],
      check_goods:[]
    }
  },
  computed:{
    // 总价
    total_price(){
      let price=0;
      this.check_goods.forEach(item => {
        price+=Number(item.num)*Number(item.goods_price);
      });
      return price;
    },
    // 全部数量
    total_num(){
      var num=0;
      this.check_goods.forEach(item => {
        num+=Number(item.num);
      })
      return num;
    } 
  },
  methods: {
    // 减少
    reduce(item){
      if(item.num<=1){
        item.num=1;
      }else{
        item.num--
      }
    },
    // 增加
    add(item){
      item.num++;
    },
    // 删除
    delete_num(item){
      if(confirm('确定删除此商品吗？')){
        this.check_goods.splice(this.check_goods.indexOf(item),1);
        this.cart_list.splice(this.cart_list.indexOf(item),1);
      }
    },
    // 全选
    check_all() {
      if(this.check_goods.length<this.cart_list.length){
        this.check_goods=[];
        this.cart_list.forEach(item => {
          this.check_goods.push(item);
        })
      }else{
        this.check_goods=[];// 清空选中的商品
      }
    },
    // 去结算
    goSubmit() {
      if(this.check_goods.length===0){
        alert('请选中商品！')
      }else{
        alert('提交成功！');
      }
    }
  },
  filters: {
    // 过滤数量都为整数
    formateNum(num){
      num=Math.floor(Number(num));
      return num;
    }
  }
}
</script>

<style scoped>
  .shopCarTable input[type='checkbox']{vertical-align:1px;}
  .shopCarTable th{padding:10px 0;text-align:center;}
  .shopCarTable td{padding:5px 0;text-align:center;}
  .total{padding-top:20px;}
  .pull-right{text-align:right;margin-top:-28px;}
</style>
