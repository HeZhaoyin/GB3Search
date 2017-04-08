<template>
	<div class="logo" id="logo">
		<img :src="loadLogoSrc()">
		<span class="logo-flag" @click="showLogoList"></span>

		<transition-group name="listfade" tag="ul">
			<li v-for="(item,index) in logoData" :key="item" v-if="showLogoFlag" @click="changeIndex(index)">
				<img :src="item.logoSrc" alt="">
			</li>
		</transition-group>
	</div>
</template>

<script>
export default {
	data(){
		return {
			logoIndex:0,
			showLogoFlag:false,
			logoData:[
				{
					logoSrc:require('../assets/bd_logo.png')
				},
				{
					logoSrc:require('../assets/google_logo.png')
				},
				{
					logoSrc:require('../assets/360_logo.png')
				}
			],
		}
	},
	methods:{
		loadLogoSrc:function(){
			return this.logoData[this.logoIndex].logoSrc;
		},
		showLogoList:function(){
			this.showLogoFlag = !this.showLogoFlag;
		},
		changeIndex:function(index){
			this.logoIndex = index;
			this.showLogoFlag = false;
			this.$emit('sendLogoIndex',this.logoIndex);
		}
	}
}
</script>

<style>
.logo{
	position: relative;
	width: 272px;
	height: 92px;
	margin: 0 auto;
}
.logo>img{
	width:272px;
}
.logo-flag{
	position: absolute;
	width: 0;
	height: 0;
	border: 8px solid;
	border-color: #000 transparent transparent transparent;
	top: 40px;
	left: 300px;
	cursor: pointer;
}
.logo>ul{
	padding: 0;
}
.logo>ul>li{
	list-style: none;
	width: 270px;
	height: 80px;
	background-color: #fff;
	border: 1px solid #ccc;
	vertical-align: middle;
	line-height: 80px;
	cursor: pointer;
}
.logo>ul>li:hover{
	background-color: #ccc;
}
.logo>ul>li>img{
	width:200px;
	vertical-align: middle;
}
.listfade-enter-active {
  transition: all .3s ease;
}
.listfade-leave-active {
  transition: all .5s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.listfade-enter, .listfade-leave-active {
  transform: translateX(10px);
  opacity: 0;
}
</style>
