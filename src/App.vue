<template>
  <div id="app">
	<logo-select></logo-select>
	<div class="search-box">
		<input type="text" class="search-input" id="searchBox" v-model="text" @keyup="getList">
		<button type="button" class="search-btn" name="button">搜索</button>
	</div>
	<div class="list">
		<transition-group name="itemfade" tag="ul" mode="out-in" v-cloak>
			<li v-for="list in resultList" :key="list">{{list[0]}}</li>
		</transition-group>
	</div>
  </div>
</template>

<script>
import logoSelect from './components/logo-select.vue'
export default {
  name: 'app',
  components:{
	  'logo-select':logoSelect
  },
  data(){
	  return {
		  text:'',
		  //https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su?wd=a&cb=
		  //https://www.google.com/complete/search?client=hp&hl=zh-TW&gs_rn=64&gs_ri=hp&tok=c3XrYVdCgKFBRZ2lMqcs0A&cp=2&gs_id=vc&q=12&xhr=t
		  url:'https://www.google.com/complete/search?client=hp&hl=zh-TW&gs_rn=64&gs_ri=hp&tok=c3XrYVdCgKFBRZ2lMqcs0A&cp=2&gs_id=vc',
		  callback:'jsonp',
		  resultList:[]
	  }
  },
  methods:{
	  getList:function(){
		  this.$http.jsonp(this.url,{params:{q:this.text},jsonp:this.callback}).then(function(res){
			  this.resultList = res.data[1];
		  })
	  }
  }
}
</script>

<style>
#app {
  font-family: Microsoft Yahei;
  text-align: center;
  margin-top: 300px;
}
body{
	background: url('./assets/bdbg.jpg');
}
.search-box{
	font-size: 0px;
}
.search-input{
	width:400px;
	height:35px;
	margin-top: 20px;
	padding: 3px 10px 3px 10px;
	border: 1px solid #ccc;
	box-sizing: border-box;
	outline: none;
	font-size: 18px;
}
.search-btn{
	width:100px;
	height:35px;
	border: 1px solid #38f;
	background-color: #38f;
	color:#fff;
	position: relative;
	top: 1px;
	outline: none;
	cursor: pointer;
	box-sizing: border-box;
}
.list{
	width:400px;
	height:30px;
	margin: 0 auto;
	padding-right: 100px;
}
.list>ul{
	margin:0;
	padding:0;
}
.list>ul>li{
	list-style: none;
	text-align: left;
	line-height: 30px;
	padding-left: 10px;
	background: #fff;
	font-size: 14px;
	transition: all .3s ease;
}
.itemfade-enter,
.itemfade-leave-active {
    opacity: 0;
}

.itemfade-leave-active {
    position: absolute;
}
</style>
