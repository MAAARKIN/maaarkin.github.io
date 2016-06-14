<template>
	<div id="app">
		<div class="top-header">
			<navbar></navbar>
			<div class="container content-header">
				<intro></intro>
			</div>
		</div>

		<div class="row">
			<div class="col s4" style="background-color: orange;">
				conteudo
			</div>
			<div class="col s8" style="background-color: purple;">
				conteudo
			</div>
		</div>

		<!-- <div class="container"> -->
			<!-- <blockquote>
				<h3>What I Do?</h3>
				<progress-bar name="marcos" :percent="60"></progress-bar>
			</blockquote> -->

		<!-- </div> -->
	</div>
</template>

<script>
	import Navbar from './components/Navbar.vue'
	import Intro from './components/Intro.vue'
	import ProgressBar from './components/ProgressBar.vue'

	export default {
		components: {
			Navbar,
			Intro,
			ProgressBar
		}
	}
</script>

<style lang="scss">
	@media (max-width: 1280px) {
		.top-header {
			min-height: 700px;
		}
	}

	.top-header {
		min-height: 750px;
		background: url(../assets/img/officer.jpg) no-repeat;
		background-repeat: no-repeat;
		background-position: center;
		background-size: cover;

		p {
			color: rgba(255, 255, 255, 0.75);
		}

		.content-header {
			padding: 1em 0em;
		}
	}

	// .container {
	// 	blockquote {
	// 		border-left: 5px solid #26a69a;
	// 	}
	// }
</style>
