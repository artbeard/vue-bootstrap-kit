<script lang="ts">
import {defineComponent} from 'vue'
import type { PropType } from 'vue'
import SlimSelect from 'slim-select'
//const SlimSelect = require('slim-select')
//import 'slim-select/styles';
export interface ISelectOptionInterface{
	text: string,
	value: string,
	placeholder?: boolean
}
export default defineComponent({
	name: 'SlimSelect',
	data(){return {
		selectEl: null as null | typeof SlimSelect
	}},
	props: {
		modelValue: {
			//type: Object as PropType<string[] | string>,
			type: [Array, String] as PropType<string[] | string>
		},
		options: {
			type: Object as PropType<ISelectOptionInterface[]>
		},
		multiple: {
			type: Boolean,
			default: () => false
		},
		closeOnSelect: {
			type: Boolean,
			default: () => true
		}
	},
	emits: ['update:modelValue'],
	computed: {
		mValue: {
			get(): string | string[]{
				return this.modelValue ?? (this.multiple ? [] : '');
			},
			set(newValue: string | string[]){
				this.$emit('update:modelValue', newValue)
			}
		},
		optionList(): ISelectOptionInterface[]
		{
			return this.options ?? [];
		}
	},
	watch: {
		optionList: {
			handler: function (newData: ISelectOptionInterface[])
			{
				if (this.selectEl instanceof SlimSelect)
				{
					this.selectEl.setData(newData);
				}
			},
			deep : true
		},
		modelValue: {
			handler: function (newData: any, oldData: any){
				if (this.selectEl instanceof SlimSelect)
				{
					//console.log(this.selectEl.getSelected());
					if (Array.isArray(newData) && newData.toString() !== oldData.toString())
					{
						//console.log('Изменено выбранное значчение', newData);
						this.selectEl.setSelected(newData);
					}
					else if (!Array.isArray(newData) && newData !== oldData)
					{
						//console.log('Изменено выбранное значчение', newData);
						this.selectEl.setSelected(!!newData ? newData : '' );
					}
				}
			},
			deep : true
		},
	},
	methods: {
		beforeChange(newV: any[]/*, oldV:any[]*/)
		{
			if (this.multiple)
			{
				this.mValue = newV.map(el => el.value);
			}
			else
			{
				this.mValue = newV.map(el => el.value)[0] ?? '';
			}
			//return false // можно запретить выбор определенного значения
		}
	},
	mounted() {
		this.selectEl = new SlimSelect({
			select: this.$refs.select,
			settings: {
				allowDeselect: true,
				closeOnSelect: this.closeOnSelect
			},
			events: {
				beforeChange: this.beforeChange
			}
		});
		this.selectEl.setData(this.optionList);
	},
	beforeUnmount() {
		if (this.selectEl instanceof SlimSelect)
			this.selectEl.destroy();
	},
	// setup(props)
	// {
	// 	// watch(props.options, (newVal) => {
	// 	// 	console.log('fromSetup', newVal)
	// 	// })
	// }
})
</script>

<template>
	<select class="form-select" ref="select" :multiple="multiple">
<!--		<option v-for="opt in optionList" :value="opt.value" :key="'_opt' + opt.value"></option>-->
	</select>
</template>

<style lang="scss">
@import '../../../node_modules/slim-select/dist/slimselect.css';
.ss-main{
	.ss-values{
		padding: 0.25rem 0.75rem 0.25rem 0.5rem;
	}
	.ss-arrow{
		opacity: 0.5;
	}
}
.ss-main.form-select, .ss-content.form-select{
	background-image: none;
	padding: 0.35em 0.1em;
}
.ss-content.form-select{
	padding: 0.5px;
	border-radius: var(--tblr-border-radius);
}

.ss-content.form-select.is-invalid{
	padding-right: 0.5em	!important;
}


</style>