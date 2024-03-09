<template>
	<teleport to="body">
		<div
			class="modal modal-blur vue-modal-overlay"
			:class="showModal ? 'fade show d-block' : ''"
			tabindex="-1"
			_style="display: block"
		>
			<div
				class="modal-dialog modal-dialog-centered 		modal-dialog-scrollable"
				:class="
					size == 'full'
						? 'modal-full-width'
						: `modal-${size}`
				"
				role="document"
			>
				<div class="modal-content shadow-lg">
					<div class="modal-header">
						<h5 class="modal-title" v-html="title"></h5>
						<button
							type="button"
							class="btn-close"
							v-if="closeButton"
							@click="onClose($event)"
						></button>
					</div>
					<div class="modal-body" :class="pad">
						<slot></slot>
					</div>

					<div
						class="modal-footer"
						v-show="footer && (cancelButton || okButton)"
					>
						<button
							type="button"
							class="btn me-auto"
							v-if="cancelButton != false"
							@click="onCanscel($event)"
						>
							{{ cancelButton }}
						</button>
						<button
							type="button"
							class="btn btn-primary"
							v-if="okButton != false"
							@click="onOk($event)"
						>
							{{ okButton }}
						</button>
					</div>
				</div>
			</div>
		</div>
	</teleport>
</template>


<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({
	name: 'Modal',
	data() {
		return {
			showModal: false,
		};
	},
	props: {
		id: {
			type: String,
			default: null,
		},
		/**
		 * Заголовок окна
		 */
		title: {
			type: String,
			default: "Заголовок окна",
		},
		/**
		 * Показывать ли footer
		 */
		footer: {
			type: Boolean,
			default: true,
		},
		/**
		 * Кнока закрытия
		 */
		closeButton: {
			type: Boolean,
			default: true,
		},
		/**
		 * Кнопка отмены
		 */
		cancelButton: {
			type: [String, Boolean],
			default: "Отмена",
		},
		/**
		 * Кнопка Ok
		 */
		okButton: {
			type: [String, Boolean],
			default: "Ok",
		},
		/**
		 * Размер
		 */
		size: {
			type: String,
			default: "sm",
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
	},
	emits: ["modalOk", "modalCansel", "modalClose", "modalShow", "modalHide"],
	methods: {
		//События кнопок
		onOk(e: Event | null = null) {
			this.hide();
			this.$emit("modalOk", e);
		},
		onCanscel(e: Event | null = null) {
			this.hide();
			this.$emit("modalCansel", e);
		},
		onClose(e: Event | null = null) {
			this.hide();
			this.$emit("modalClose", e);
		},
		//Методы
		show(e: Event | null = null) {
			this.showModal = true;
			return new Promise((resolve) => {
				this.$emit("modalShow", e);
				resolve(true);
			});
		},
		hide(e: Event | null = null) {
			this.showModal = false;
			return new Promise((resolve) => {
				this.$emit("modalHide", e);
				resolve(true);
			});
		},
		toggle() {
			if (this.showModal) this.hide();
			else this.show();
		},
	},
	created() {
		// if (
		//    typeof this.$vueModals != 'undefined' &&
		//    this.id != null &&
		//    !(this.id in this.$vueModals)
		// ) {
		//    this.$vueModals[this.id] = this;
		// }
	},
	unmounted() {
		// if (
		//    typeof this.$vueModals != 'undefined' &&
		//    this.id != null &&
		//    !(this.id in this.$vueModals)
		// ) {
		//    delete this.$vueModals[this.id];
		// }
	},
});
</script>

<style lang="scss">
.vue-modal-overlay {
	background: rgba(0, 0, 0, 0.5);
}
/*.vue-modal {}*/
</style>