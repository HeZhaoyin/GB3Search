<template>
  <div id="app">
	<logo-select @sendLogoIndex="getLogoIndex"></logo-select>
	<div class="search-box">
		<input type="text" class="search-input" id="searchBox" v-model="text" @keyup="getList(searchIndex)">
		<button type="button" class="search-btn" name="button" @click="searchStart(text)">搜索</button>
	</div>
	<div class="list">
		<transition-group name="itemfade" tag="ul" mode="out-in" v-cloak>
			<li v-for="list in resultList" :key="list" @click="changeText(list)">{{list}}</li>
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
		  searchIndex:0,
		  text:'',
		  //https://www.baidu.com/s?wd=a&rsv_spt=1&rsv_iqid=0xf0c4fb19000d0fe3&issp=1&f=8&rsv_bp=0&rsv_idx=2&ie=utf-8&tn=baiduhome_pg&rsv_enter=0&rsv_sug3=1&rsv_sug1=1&rsv_sug7=100&inputT=1043&rsv_sug4=1044
		  resultList:[],
		  searchList:[
			  {
				url:'https://sp0.baidu.com/5a1Fazu8AA54nxGko9WTAnF6hhy/su'
			  },
			  {
				url:'https://www.google.com/complete/search?client=hp&hl=zh-TW&gs_rn=64&gs_ri=hp&tok=c3XrYVdCgKFBRZ2lMqcs0A&cp=2&gs_id=vc'
			},
			{
				url:'https://sug.so.360.cn/suggest'
			}
		  ]
	  }
  },
  methods:{
	  getList:function(index){
		  switch (index) {
			case 0:
				this.$http.jsonp(this.searchList[0].url,{params:{wd:this.text},jsonp:'cb'}).then(function(res){
					this.resultList = res.data.s;
				});
				break;
		  	case 1:
				this.$http.jsonp(this.searchList[1].url,{params:{q:this.text},jsonp:'jsonp'}).then(function(res){
					this.resultList = [];
					for (var i = 0; i < res.data[1].length; i++) {
						this.resultList.push(res.data[1][i][0])
					}
				});
		  		break;
			case 2:
				this.$http.jsonp(this.searchList[2].url,{params:{word:this.text}}).then(function(res){
					this.resultList = res.data.s;
				});
				break;
		  }

	  },
	  getLogoIndex:function(index){
		  this.searchIndex = index;
		  this.text = '';
		  this.getList(this.searchIndex);
	  },
	  changeText:function(text){
		  this.text = text;
		  this.resultList = [];
		  this.searchStart(text);
	  },
	  searchStart:function(text){
		  switch (this.searchIndex) {
		  	case 0:
				window.open('https://www.baidu.com/s?wd=' + text);
		  		break;
			case 1:
				window.open('https://www.google.com/search?site=&source=hp&q=' + text);
				break;
		  }
	  }
  }
}
</script>

<style>
#app {
  font-family: Microsoft Yahei;
  text-align: center;
  margin-top: 250px;
}
body{
	background: url('./assets/bdbg.jpg');
}
.search-box{
	font-size: 0px;
}
.search-input{
	width:430px;
	height:45px;
	margin-top: 20px;
	padding: 3px 10px 3px 10px;
	border: 1px solid #ccc;
	box-sizing: border-box;
	outline: none;
	font-size: 18px;
}
.search-btn{
	width:100px;
	height:45px;
	border: 1px solid #38f;
	background-color: #38f;
	color:#fff;
	position: relative;
	top: -1px;
	outline: none;
	cursor: pointer;
}
.list{
	width:430px;
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
	box-sizing: border-box;
	cursor: pointer;
}
.list>ul>li:hover{
	background-color: #ccc;
}
.itemfade-enter,
.itemfade-leave-active {
    opacity: 0;
}

.itemfade-leave-active {
    position: absolute;
}
</style>
