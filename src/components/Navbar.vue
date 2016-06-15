<template>
	<nav>
		<div class="nav-wrapper">
			<a href="/" class="brand-logo">
				<img class="logo" src="assets/img/logoMF1.png">
			</a>
			<a href="#" data-activates="mobile-demo" class="button-collapse"><i class="material-icons">menu</i></a>
			<ul class="right hide-on-med-and-down">
				<li><a href="#about">Resumo</a></li>
				<!-- <li><a href="#">Link 2</a></li> -->
				<!-- <li><a href="#">Link 3</a></li> -->
				<!-- <li><a href="#">Link 3</a></li> -->
			</ul>
			<ul class="side-nav" id="mobile-demo">
				<li><a href="#about">Resumo</a></li>
				<!-- <li><a href="#">Link 2</a></li> -->
				<!-- <li><a href="#">Link 3</a></li> -->
				<!-- <li><a href="#">Link 4</a></li> -->
			</ul>
		</div>
	</nav>
</template>

<script>
	export default {
		ready() {
			console.log("created")
			$(".button-collapse").sideNav();
			$('.scrollspy').scrollSpy();
		}
	};
</script>

<style lang="scss" scoped>
	nav {
		background-color: rgba(62, 54, 59, 0.36);

		.nav-wrapper {
			padding: 0 2%;
		}

		.logo {
			width: 200px;
			height: 100px;
			margin-top: 5px;
		}
	}
</style>