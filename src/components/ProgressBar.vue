<template>
	<div> 
		<div id="{{name}}" class="progress-bar" :style="size">{{percent}}% 
		</div> 
	</div>
	<br>
	{{ name }}
</template>

<script>
	export default {
		data() {
			return {

			};
		},

		computed: {
			size: function(argument) {
				return "width: "+this.percent+"%";
			} 
		},

		props: {
			name: String,
			percent: Number
		}

	};
</script>

<style lang="css" scoped>
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