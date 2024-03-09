<script lang="ts">
import {defineComponent} from 'vue'
//import 'jodit/es2021/jodit.min.css'
import { Jodit } from 'jodit/es2021/jodit.js'
import { Jodit as IJodit } from "jodit/types/jodit.d.ts";
export default defineComponent({
	name: 'Jodit',
	data:()=>({
		editor: null as null | IJodit
	}),
	props: {
		modelValue: { type: String, required: true },
		// buttons: { type: Array, default: null },
		// extraButtons: { type: Array, default: null },
		// config: { type: Object, default: () => ({}) },
		// plugins: { type: Array, default: () => [] }
	},
	watch: {
		modelValue(newValue: string)
		{
			if (this.editor && (this.editor as IJodit).value !== newValue) (this.editor as IJodit).value = newValue
		}
	},
	emits: ['update:modelValue'],
	mounted() {

		this.editor = Jodit.make(this.$el, {
			buttons: [
				'bold',
				'italic',
				'underline',
				'strikethrough',
				'ul',
				'ol',
				//'image',
				//'table',
				'link',
				'|',
				'eraser',
				'source',
				'fullsize',
				'preview'
			],
			toolbarAdaptive: false
		});
		(this.editor as IJodit).value = this.modelValue;
		(this.editor as IJodit).events.on('change', (newValue: string) =>
			this.$emit('update:modelValue', newValue)
		)
	},
	beforeMount() {
		if (this.editor)
		{
			this.editor.destruct();
		}
	}
})
</script>

<template>
	<textarea></textarea>
</template>

<style lang="scss">
@import 'jodit/es2021/jodit.css';
</style>