<template>
	<div class="progress"> 
		<div class="progress-bar" :style="size">{{percent}}%</div> 
	</div>
</template>

<script>
	export default {
		computed: {
			size: function(argument) {
				return "width: "+this.percent+"%";
			} 
		},

		props: {
			percent: Number
		}

	};
</script>

<style lang="css" scoped>

	.progress {
		height: 20px;
		overflow: hidden;
		background-color: #d8d8d8;
		border-radius: 4px;
		-webkit-box-shadow: inset 0 1px 2px rgba(0,0,0,.1);
		box-shadow: inset 0 1px 2px rgba(0,0,0,.1);
	}

	.progress-bar {
		float: left;
		width: 0;
		height: 100%;
		font-size: 12px;
		line-height: 20px;
		color: #fff;
		text-align: center;
		background-color: #337ab7;
		-webkit-box-shadow: inset 0 -1px 0 rgba(0,0,0,.15);
		box-shadow: inset 0 -1px 0 rgba(0,0,0,.15);
		-webkit-transition: width .6s ease;
		-o-transition: width .6s ease;
		transition: width .6s ease;
	}
</style>