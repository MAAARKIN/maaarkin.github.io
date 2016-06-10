<template>
	<div id="app" class="top-header">
		<navbar></navbar>
		<div class="container">
			<div class="intro-header">
				<p>
					Hello,<br>
					I'm Marcos Filho. <br>
					A Full-stack Developer.<br>
					Based in Brazil<br>
				</p>

				<p>Under Construction.</p>
			</div>
		</div>
	</div>
</template>

<script>
	import Navbar from './components/Navbar.vue'

	export default {
		components: {
			Navbar
		}
	}
</script>

<style>
	@media (max-width: 1280px) {
		.top-header {
			min-height: 600px;
		}
	}

	.top-header {
		padding: 1em 0em;
		min-height: 650px;
	}
	.top-header {
		background: url(../assets/img/officer.jpg) no-repeat;
		background-repeat: no-repeat;
		background-position: center;
		background-size: cover;
	}

	.intro-header {
		font-family: 'Vollkorn', serif;
		font-size: 3em;
		margin: 0;
		line-height: 1.3em;
	}

	body {
		font-family: Helvetica, sans-serif;
	}

	p {
		color: rgba(255, 255, 255, 0.75);
	}
</style>
