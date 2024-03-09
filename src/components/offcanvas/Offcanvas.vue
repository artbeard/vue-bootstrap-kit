<template>
	<teleport to="body">
		<div class="offcanvas"
		     :class="[
				  {'show': showModal},
				  sizeClass,
				  positionClass
		     ]"
		     tabindex="-1"
		     aria-modal="true"
		     role="dialog"
		>

			<div class="offcanvas-header">
				<h2 class="offcanvas-title" v-html="title"></h2>
				<button type="button" class="btn-close text-reset" aria-label="Close" @click="hide"></button>
			</div>
			<div class="offcanvas-body">
				<div>
					<slot></slot>
				</div>
				<div class="mt-3" v-if="okButton !== false">
					<button class="btn btn-primary" type="button" @click="onOkPressed">
						{{okButton}}
					</button>
				</div>
			</div>

		</div>
		<div class="offcanvas-backdrop fade"
		     v-if="showModal && overlay"
		     :class="[
				  {'show': showModal},
		     ]"
		     @click="hide"
		></div>
	</teleport>
</template>

<script lang="ts">
import {defineComponent, PropType} from 'vue'

export default defineComponent({
	name: 'Offcanvas',
	data:() => ({
		showModal: false as boolean
	}),
	props: {
		/**
		 * Заголовок окна
		 */
		title: {
			type: String,
			default: 'Заголовок',
		},
		/**
		 * Кнопка Ok
		 */
		okButton: {
			type: [String, Boolean],
			default: 'Ok',
		},
		/**
		 * Размер
		 */
		size: {
			type: String,
			default: 'sm',
		},
		/**
		 * Остпур от краев экрана
		 */
		pad: {
			type: String,
			default: '',
		},
		/**
		 * Оверлей
		 */
		overlay: {
			type: Boolean,
			default: true,
		},
		/**
		 * Позиция
		 */
		position: {
			type: String,
			default: 'start',
		},

		onOk: {
			type: Object as PropType<Promise<any>>,
			default: () => Promise.resolve(true)
		}
	},

	emits: ["modalOk", "modalCansel", "modalClose", "modalShow", "modalHide"],
	computed: {
		//offcanvas-size-md
		sizeClass()
		{
			switch (this.size)
			{
				case 'md': return 'offcanvas-size-md';
				case 'lg': return 'offcanvas-size-lg';
				case 'xl': return 'offcanvas-size-xl';
				case 'xxl': return 'offcanvas-size-xxl';
				default: return  '';
			}
		},
		positionClass()
		{
			switch (this.position)
			{
				case 'top': return 'offcanvas-top';
				case 'bottom': return 'offcanvas-bottom';
				case 'end': return 'offcanvas-end';
				case 'start': return 'offcanvas-start';
				default: return  'offcanvas-start';
			}
		}
	},
	methods: {
		show(e: Event | null = null) {
			this.showModal = true;
			return new Promise((resolve) => {
				this.$emit("modalShow", e);
				resolve(true);
			});
		},

		hide(e: Event | null = null) {

			//todo добавлять класс hiding при закрытии - для плавного закрывания

			this.showModal = false;
			return new Promise((resolve) => {
				this.$emit("modalHide", e);
				resolve(true);
			});
		},

		onOkPressed()
		{
			this.onOk
				.then(() => {
					this.hide()
				})
		}
	}
})
</script>


<style scoped lang="scss">
.offcanvas {
	position: fixed;
	bottom: 0;
	z-index: var(--tblr-offcanvas-zindex);
	display: flex;
	flex-direction: column;
	max-width: 100%;
	color: var(--tblr-offcanvas-color);
	visibility: hidden;
	background-color: var(--tblr-offcanvas-bg);
	background-clip: padding-box;
	outline: 0;
	box-shadow: var(--tblr-offcanvas-box-shadow);
	transition: var(--tblr-offcanvas-transition);
}


.offcanvas{
	--tblr-offcanvas-zindex: 1045;
	--tblr-offcanvas-width: 400px;
	--tblr-offcanvas-height: 30vh;
	--tblr-offcanvas-padding-x: 1.5rem;
	--tblr-offcanvas-padding-y: 1.5rem;
	--tblr-offcanvas-color: var(--tblr-body-color);
	--tblr-offcanvas-bg: var(--tblr-bg-surface);
	--tblr-offcanvas-border-width: var(--tblr-border-width);
	--tblr-offcanvas-border-color: var(--tblr-border-color);
	--tblr-offcanvas-box-shadow: 0 0.125rem 0.25rem rgba(0, 0, 0, 0.075);
	--tblr-offcanvas-transition: transform 0.3s ease-in-out;
	--tblr-offcanvas-title-line-height: 1.4285714286;
}

.offcanvas-size-sm, .offcanvas-size-xxl{
	--tblr-offcanvas-width: 100vw;
	--tblr-offcanvas-height: 100vh;
}
.offcanvas-size-md{
	--tblr-offcanvas-width: 50vw;
	--tblr-offcanvas-height: 50vh;
}
.offcanvas-size-lg, .offcanvas-size-xl{
	--tblr-offcanvas-width: 75vw;
	--tblr-offcanvas-height: 75vh;
}

//.offcanvas-sm, .offcanvas-md {
//	background-color: white;
//}


.offcanvas.offcanvas-start {
	top: 0;
	left: 0;
	width: var(--tblr-offcanvas-width);
	border-right: var(--tblr-offcanvas-border-width) solid var(--tblr-offcanvas-border-color);
	transform: translateX(-100%);
}
.offcanvas.offcanvas-end {
	top: 0;
	right: 0;
	width: var(--tblr-offcanvas-width);
	border-left: var(--tblr-offcanvas-border-width) solid var(--tblr-offcanvas-border-color);
	transform: translateX(100%);
}
.offcanvas.offcanvas-top {
	top: 0;
	right: 0;
	left: 0;
	height: var(--tblr-offcanvas-height);
	max-height: 100%;
	border-bottom: var(--tblr-offcanvas-border-width) solid var(--tblr-offcanvas-border-color);
	transform: translateY(-100%);
}
.offcanvas.offcanvas-bottom {
	right: 0;
	left: 0;
	height: var(--tblr-offcanvas-height);
	max-height: 100%;
	border-top: var(--tblr-offcanvas-border-width) solid var(--tblr-offcanvas-border-color);
	transform: translateY(100%);
}

.offcanvas.hiding, .offcanvas.show, .offcanvas.showing {
	visibility: visible;
}

.offcanvas.show:not(.hiding), .offcanvas.showing {
	transform: none;
}

</style>
