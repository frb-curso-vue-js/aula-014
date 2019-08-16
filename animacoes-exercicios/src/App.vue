<template>
	<div id="app" class="container-fluid">
		<h1>Animações</h1>
		<hr>
		<b-button variant="info" class="mb-3" @click="exibir = !exibir">Mostrar mensagem</b-button>

		<transition name="fade" appear>
			<b-alert variant="info" show v-if="exibir">{{msg}}</b-alert>
		</transition>

		<transition name="slide" appear>
			<b-alert variant="secondary" show v-show="exibir">{{msg2}}</b-alert>
		</transition>
		<hr>
		<transition enter-active-class="animated bounce" leave-active-class="animated shake" appear>
			<b-alert variant="secondary" show v-show="exibir">{{msg3}}</b-alert>
		</transition>
		<hr>
		<b-select v-model="tipoAnimacao" class="mb-4" appear>
			<option value="fade">Fade</option>
			<option value="slide">Slide</option>
		</b-select>
		<transition :name="tipoAnimacao" mode="out-in" appear>
			<b-alert variant="secondary" show v-if="exibir" key="primeiro">{{msg4}} {{tipoAnimacao}}</b-alert>
			<b-alert variant="warning" show v-else key="segundo">{{msg4}} {{tipoAnimacao}}</b-alert>
		</transition>

		<hr>
		<button @click="exibirJS = !exibirJS">Alternar</button>
		<transition
		:css="false"
			@before-enter="beforeEnter"
			@enter="enter"

			@before-leave="beforeLeave"
			@leave="leave"
		>
			<div class="caixa" v-if="exibirJS"></div>
		</transition>
		<hr>
		<b-button variant="warning" @click="componenteSelecionado = 'AlertaAdvertencia'">Advertência</b-button>
		<br>
		<b-button variant="info" @click="componenteSelecionado = 'AlertaInfo'">Info</b-button>
		<hr>
		<transition name="fade" mode="out-in" appear>
			<component :is="componenteSelecionado"></component>
		</transition>
		<hr>
		<b-button class="success" @click="adicionarAluno">Adicionar</b-button>
			<transition-group name="slide" tag="div">
				<b-list-group v-for="(aluno, index) in alunos" :key="aluno">
					<b-list-group-item @click="removerAluno(index)">{{aluno}}</b-list-group-item>
				</b-list-group>
			</transition-group>
		

	</div>
</template>

<script>
import AlertaAdvertencia from './AlertaAdvertencia.vue'
import AlertaInfo from './AlertaInfo.vue'

export default {
	components: {
		AlertaAdvertencia,
		AlertaInfo
	},
	data() {
		return {
			alunos: ['Roberto', 'Julia', 'Teresa', 'Paulo', 'Alexandre', 'Romeu', 'Carlos'],
			msg: 'Mensagem para usuário - FADE',
			msg2: 'Mensagem para usuário - SLIDE',
			msg3: 'Mensagem para usuário - CUSTOM',
			msg4: 'Mensagem para usuário -',
			exibir: false,
			exibirJS: true,
			tipoAnimacao: 'fade',
			larguraBase: 0,
			componenteSelecionado: 'AlertaInfo'
		}
	},
	methods: {
		adicionarAluno() {
			this.alunos.push(Math.random().toString(36).substring(2))
		},
		removerAluno(indice) {
			this.alunos.splice(indice, 1)
		},
		beforeEnter(el) {
			this.larguraBase = 0;
			el.style.width = `${this.larguraBase}px`
		},
		animar(el, done, negativo) {
			let rodada = 1
			const temporizador = setInterval(() => {
				const novaLargura = this.larguraBase + (negativo ? -rodada * 10 : rodada * 10)
				el.style.width = `${novaLargura}px`
				rodada++
				if (rodada > 30) {
					clearInterval(temporizador)
					done()
				}
			}, 20)
		},
		enter(el, done) {
			this.animar(el, done, false)
		},
/* 		afterEnter(el) {
			//console.log('afterEnter')
		},
		enterCancelled(el) {
			//console.log('enterCancelled')
		}, */
		beforeLeave(el) {
			this.larguraBase = 300;
			el.style.width = `${this.larguraBase}px`
		},
		leave(el, done) {
			this.animar(el, done, true)
		},
/* 		afterLeave(el) {
			//console.log('afterLeave')
		},
		leaveCancelled(el) {
			//console.log('leaveCancelled')
		}, */
	}

}
</script>

<style>
#app {
	font-family: 'Avenir', Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
	margin-top: 60px;
	font-size: 1.5rem;
}


.caixa {
	height: 100px;
	width: 300px;
	margin: 30px auto;
	background-color: lightgreen;
	border-radius: 40px;
}


.fade-enter, .fade-leave-to {
	opacity: 0;
}

.fade-enter-active, .fade-leave-active {
	transition: opacity 1s;
}

@keyframes slide-in {
	from { transform: translateY(40px); }
	to { transform: translateY(0); }
}

@keyframes slide-out {
	from { transform: translateY(0); }
	to { transform: translateY(40px); }
}

.slide-enter-active {
	animation: slide-in 2s ease;
	transition: opacity 2s;
}

.slide-leave-active {
	position: absolute;
	width: 100%;
	animation: slide-out 2s ease;
	transition: opacity 2s;
}

.slide-enter, .slide-leave-to {
	opacity: 0;
}

.slide-move {
	transition: transform 1s;
}

/* .fade-enter {
	opacity: 0;
}

.fade-enter-active {
	transition: opacity 1s;
}

.fade-enter-to {
	opacity: 1;
}

.fade-leave {
	opacity: 1;
}

.fade-leave-active {
	transition: opacity 1s;
}

.fade-leave-to {
	opacity: 0;
} */

</style>
