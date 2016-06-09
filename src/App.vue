<template>
	<div id="app" >
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
	.intro-header {
		font-family: 'Vollkorn', serif;
		font-size: 3em;
		margin: 0;
		line-height: 1.3em;
	}

	body {
		font-family: Helvetica, sans-serif;
	}
</style>
