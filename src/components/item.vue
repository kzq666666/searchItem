<template>
  <div id="itemPage">
    <div class="header">
      <span>商品信息：</span>
      <input type="text" v-model="itemMessage">
      <button @click="submit">提交</button>
    </div>
    <div class="body">
      <table border="1" v-if="data">
        <thead id="tableHead">
          <th id="abiid">id</th>
          <th id="mainName">商品名称</th>
          <th id="subTitle">描述</th>
          <th>价格</th>
          <th>展示</th>
        </thead>
        <tbody v-for="(item,index) in data" class="tableBody" :class="{odd:index%2==0}">
          <td>{{item.abiid}}</td>
          <td>{{item.mainname}}</td>
          <td>{{item.subtitle}}</td>
          <td>{{item.price}}</td>
          <td><img :src="item.img_url" alt="" title="123"></td>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import qs from 'qs'
import md5 from 'js-md5'
export default {
  data() {
    return {
      itemMessage:'',
      token:null,
      data:null
    }
  },
  methods: {
    submit(){
      this.$http.get('apis/goods/search/1?a=a&key='+this.itemMessage,{headers:{'Authorization':'Bearer '+ this.token}}).then(
        (res)=>{
          this.data = res.data.data;
        }
      )
    },
    getToken(){
      var _appid='sunrise_member';
      var	_appsecret='e1d0b361201e4324b37c968fb71f0d3c';
      var _timestamp=Date.parse((new Date()).toUTCString())/1000;
      var _nonce=Math.round(Math.random()*10000);
      var _array=new Array(_nonce,_appsecret,_timestamp+'');
      _array.sort();
      var tmp=_array.join('');
      var _signature=md5(tmp).toUpperCase();
      var rq={
          appid:_appid,
          appsecret:_appsecret,
          timestamp:_timestamp,
          signature:_signature,
          nonce:_nonce
      };
       this.$http.post('/apis'+'/sys/token',qs.stringify(rq)).then(
         (res)=>{
           this.token = res.data.data.token;
         }
       )
    }
    
  },
  created() {
    this.getToken();
  },
}
</script>

<style scoped>
#itemPage{
  width: 100%;
  height: 100%;
  background: linear-gradient(to right, #70e1f5, #ffd194);
}

.header{
  height: 6rem;
  text-align: center;
  display: flex;
  align-items: center;
  justify-content: center;

}
.header span{
  font-size: 1.2rem;
}
.header input{
  width: 45%;
  padding: 4px;
  font-size: 1rem;
}
.header button{
  margin-left: 1rem;
  width: 15%;
  height: 1.9rem;
  background: #F56C6C;
  border: none;
  cursor: pointer;
  text-align: center;
  line-height: 1.5rem;
  font-size: 1.1rem;
  border-radius: 3px;
}
.body{
  width: 100%;
  height:calc(100% - 6rem);
  overflow-x: hidden;
  overflow-y: scroll;
}
table{
  width: 100%;
  height: 100%;
  text-align: center;
  border-collapse: collapse;
  border: 1px solid black;
  table-layout:fixed; 
}
#abiid{
  width: 20%;
}
#mainName{
  width: 25%;
  height: 2rem;
  overflow: hidden;
}
#subTitle{
  width: 30%;
  /* padding: 2px; */
}
td{
  overflow:hidden;
  /* word-break:keep-all; */
  white-space: nowrap;
  text-overflow:ellipsis
}
td:hover{
  overflow: hidden;
  white-space: normal;
  text-overflow: unset;
}
table img{
  width: 100%;
}
</style>