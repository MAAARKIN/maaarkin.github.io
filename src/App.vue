<template>
	<div id="app">
		<header>
			<div class="top-header">
				<navbar></navbar>
				<div class="container content-header">
					<intro></intro>
				</div>
			</div>
		</header>
		<main>
			<div class="container">
				
				<div class="row" id="about">
					<div class="col s12 m6">

						<h3>Resumo</h3>

						<p align="justify">
							Sou graduado em Sistemas de Informações, busco continuar estudando e melhorando minha área de
							programação sempre que possível. Utilizo Java como plataforma principal de trabalho,
							porém tenho o desenvolvimento para web como foco principal.
						</p>
						<p align="justify">
							Atualmente procuro estar antenado a vários frameworks para web a fim de me manter atualizado.
							Tenho como hobby o estudo de front-end e mobile cross-plataform.
							Gosto de utilizar minhas horas vagas para estudar, jogar muito e curtir minha família.
						</p>
					</div>
					<div class="col s12 m6">
						<h3>Competências</h3>
						<h6>Java</h6>
						<progress-bar :percent=70></progress-bar>
						<h6>Javascript</h6>
						<progress-bar :percent=40></progress-bar>
						<h6>HTML</h6>
						<progress-bar :percent=50></progress-bar>
						<h6>CSS</h6>
						<progress-bar :percent=50></progress-bar>
					</div>
				</div>
			</div>
		</main>

		<footer class="page-footer">
			<div class="footer-copyright">
				<div class="container">
					<div class="row">
						<div class="col l5 s6">
							Developed by Marcos Filho
						</div>
						<div class="col l7 s6">
							<a href="https://linkedin.com/in/marcos-filhow" target="_blank" class="right" style="margin-right: 15px;">
								<i class="fa fa-linkedin-square fa-3x hide-on-small-only" aria-hidden="true"></i>
								<i class="fa fa-linkedin-square fa-2x hide-on-med-and-up" aria-hidden="true"></i>
							</a>
							<a href="https://github.com/MAAARKIN" target="_blank" class="right" style="margin-right: 15px;">
								<i class="fa fa-github fa-3x hide-on-small-only" aria-hidden="true"></i>
								<i class="fa fa-github fa-2x hide-on-med-and-up" aria-hidden="true"></i>
							</a>
						</div>
					</div>
				</div>
			</div>
		</footer>
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

	footer.page-footer {
		padding-bottom: 10px;
		background-color: #2c3e50;
	}

	footer {
		a {
			color: #e0e0e0;
		}
	}

	.row {
		margin-bottom: 0px;
	}

	body {
		background-color: #ececec;
	}
</style>
