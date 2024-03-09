<template>
	<div class="table-responsive" style="overflow-x: scroll;">
		<table class="table mb-0" style="_width: 1920px;">
			<thead>
				<tr>
					<template v-for="(tblHead, index) in _tableHeaders" :key="tblHead">
						<slot :name="'th' + index" :head-item="tblHead">
							<th>{{ tblHead }}</th>
						</slot>
					</template>
					
				</tr>
			</thead>
			<tbody>
				<tr v-for="(dataItm, _index) in _tableData" :key="_index">
					<td v-for="(field, index) in _tableFields" :key="index">
						{{ dataItm[field] ?? '' }}
					</td>
				</tr>
				<!-- <tr>
					<td style="left: 0; position: sticky;">ceil_1</td>
					<td style="left: 200px; position: sticky;">ceil_2</td>
					<td>ceil_3</td>
					<td>ceil_4</td>
					<td>ceil_5</td>
					<td>ceil_6</td>
					<td>ceil_7</td>
					<td>ceil_8</td>
					<td>ceil_9</td>
					<td>ceil_10</td>
				</tr> -->
			</tbody>
		</table>
	</div>
</template>
<script lang="ts">
import { defineComponent } from 'vue';
import type { PropType } from 'vue'

const VueTableHead = defineComponent({
	name: 'VueTableHead',
	props: {
		tableData: Object as PropType<any[]>,
		tableFields: Object as PropType<string[]>,
		tableHeaders: Object as PropType<string[]>,
	},
	computed: {
		_tableHeaders(): string[]
		{
			return this.tableHeaders ?? [];
		},
		_tableData(): any[]
		{
			return this.tableData ?? [];
		},
		_tableFields(): string[]
		{
			return this.tableFields ?? [];
		}
	},
	created(){
		console.log(this._tableHeaders, this._tableFields, this._tableData)
	},
	mounted() {
		
	},
})
export default VueTableHead;
export type IVueTableHead = typeof VueTableHead;
</script>
<style lang="scss"></style>