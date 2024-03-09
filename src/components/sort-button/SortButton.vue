<template>
	<button class="btn btn-sm btn-outline-default" @click.prevent="customClick">
		<i class="ti ti-selector" v-if="orderModel.field != field"></i>
		<template v-else>
			<i class="ti ti-sort-ascending" v-if="direction == 'asc'"></i>
			<i class="ti ti-sort-descending" v-else ></i>
		</template>
	</button>
</template>
<script lang="ts">
import { defineComponent } from 'vue'
import type { PropType } from 'vue'
export type TSortButtonDirection = 'asc' | 'desc';
export interface ISortButtonOrder {
	field: string,
	direction: TSortButtonDirection
}

export default defineComponent({
	name: 'SortButton',
	data() {
		return {
			direction: 'desc' as TSortButtonDirection
		}
	},
	props: {
		modelValue: {
			type: Object as PropType<ISortButtonOrder>,
			default: () => ({ field: '', direction: 'desc'}) 
		},
		
		field: {
			type: String,
			default: ''
		},

		defaultSortDirection: {
			type: String,
			default: 'desc'
		}
	},
	computed: {
		orderModel: {
			get():ISortButtonOrder { return this.modelValue },
			set(newValue: ISortButtonOrder) { this.$emit('update:modelValue', newValue) }, 
		},
		defDir()
		{
			return this.defaultSortDirection;
		}
	},
	emits: ['update:modelValue'],
	methods: {
		customClick()
		{
			if (this.modelValue.field == this.field)
			{
				this.direction = this.direction == 'asc' ? 'desc' : 'asc';
			}
			else
			{
				this.direction = this.defDir as TSortButtonDirection;
			}
			this.orderModel = {
				field: this.field,
				direction: this.direction
			};
		}	
	},
})
</script>
