<template>
	<nav>
		<div class="nav-wrapper">
			<a href="#!" class="brand-logo">MF Logo</a>
			<a href="#" data-activates="mobile-demo" class="button-collapse"><i class="material-icons">menu</i></a>
			<ul class="right hide-on-med-and-down">
				<li><a href="#">Link 1</a></li>
				<li><a href="#">Link 2</a></li>
				<li><a href="#">Link 3</a></li>
				<li><a href="#">Link 3</a></li>
			</ul>
			<ul class="side-nav" id="mobile-demo">
				<li><a href="#">Link 1</a></li>
				<li><a href="#">Link 2</a></li>
				<li><a href="#">Link 3</a></li>
				<li><a href="#">Link 4</a></li>
			</ul>
		</div>
	</nav>
</template>

<script>
	export default {

		data() {
			return {

			};
		},

		ready() {
			console.log("created")
			$(".button-collapse").sideNav();
		}
	};
</script>

<style lang="css" scoped>
	nav .nav-wrapper {
		padding: 0 2%;
	}
</style>