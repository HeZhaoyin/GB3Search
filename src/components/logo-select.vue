<template>
	<div class="logo" id="logo">
		<img :src="logoSrc">
	</div>
</template>

<script>
export default {
	data(){
		return {
			logoSrc:require('../assets/bd_logo.png')
		}
	}
}
</script>

<style>
</style>
