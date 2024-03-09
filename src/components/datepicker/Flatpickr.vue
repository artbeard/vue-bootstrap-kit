<script lang="ts">
import {defineComponent} from 'vue'
import flatpickr from 'flatpickr';
import type Instance from 'flatpickr';
import { Russian } from "flatpickr/dist/l10n/ru.js"
import ('flatpickr/dist/themes/material_blue.css');
export default defineComponent({
	name: "Flatpickr",
	data() {
		return {
			datePicker: {} as Instance.Instance,
		}
	},
	props: {
		modelValue: {
			//type: Object as PropType<string[] | string>,
			type: String
		},
		format: {
			type: String,
			default: 'Y-m-d'
		},
		locale: {
			type: String,
			default: 'en'
		}
	},
	emits: ['update:modelValue'],
	computed: {
		mValue: {
			get(): string | string[]{
				return this.modelValue ?? '';
			},
			set(newValue: string | string[]){
				this.$emit('update:modelValue', newValue)
			}
		},
		_format(){
			return this.format;
		},
		_locale(){
			return this.locale;
		},
	},
	mounted() {
		const config:any = {
			//locale: Russian,
			dateFormat: this._format,
		};
		if (this._locale && this._locale == 'ru')
		{
			config.locale = Russian;
		}
		this.datePicker = flatpickr(this.$refs.datePickerElement as HTMLInputElement, config);
	},
	beforeUnmount() {
		this.datePicker.destroy();
	}
})
</script>

<template>
	<input type="date" v-model="mValue" ref="datePickerElement" placeholder="Select a date">
</template>

<style scoped lang="scss">
//import("flatpickr/dist/themes/dark.css");
//@import '../../../node_modules/flatpickr/dist/themes/material_blue.css';

</style>