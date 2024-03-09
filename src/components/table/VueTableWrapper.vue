<template>
	<div class="table-responsive" style="overflow-x: scroll;" ref="componentWrapper">
		<table class="table mb-0 vue-table"
			:style="{width: widthTable}"
			:class="_tableClass"
			ref="mainTable">
			<thead class="vue-table_head">
				<slot name="tableHead" _tableHeaders="_tableHeaders">
					<!-- <tr v-for="(tblRows, tblRowIndex) in _tableHeaders" :key="'tblRowIndex' + tblRowIndex">
						<th v-for="(headCol, colHeadNumber) in tblRows" :key="'colHeadNumber' + colHeadNumber">
							{{ headCol.label }}
						</th>
					</tr> -->
				</slot>
			</thead>
			<tbody class="vue-table_body">
				<slot name="tableBody">
					<tr>
						<td>...</td>
					</tr>
				</slot>
			</tbody>
		</table>
	</div>
</template>
<script lang="ts">
import { defineComponent } from 'vue';
import type { PropType } from 'vue'

interface IClassName{
	[key: string]: any
}

const VueTableWrapper = defineComponent({
	name: 'VueTableWrapper',
	props: {
		//tableData: Object as PropType<any[]>,
		//tableFields: Object as PropType<string[]>,
		tableHeaders: Object as PropType<ITableHeadCol[][]>,
		stikyHeader: {
			type: Boolean,
			default: false
		},
		stikyColumns: {
			type: Number,
			default: 0
		},
		selectableColumns: {
			type: Boolean,
			default: false
		},
		selectableRows: {
			type: Boolean,
			default: false
		},
		widthTable: {
			type: String,
			default: '100%'
		},
		tableClass: {
			type: String,
			default: ''
		},
	},
	data() {
		return {
			currentIndex: null as null | number,
			showStickyHeader: false
		}
	},
	watch: {
		currentIndex(val: null | number)
		{
			((this.$refs.mainTable as HTMLElement).querySelectorAll('td.vue-table-ceil_selected, th.vue-table-ceil_selected') as NodeListOf<HTMLElement>)
				.forEach((el: HTMLElement) => (el as HTMLElement).classList.remove('vue-table-ceil_selected'));
			if (val !== null)
			{
				((this.$refs.mainTable as HTMLElement).querySelectorAll(`td[data-ceil-index="${val}"], th[data-ceil-index="${val}"]`) as NodeListOf<HTMLElement>)
					.forEach((el: HTMLElement) => (el as HTMLElement).classList.add('vue-table-ceil_selected'));
			}
		}
	},
	computed: {
		_tableClass(): any
		{
			const cl: IClassName = {
				'vue-table_sticky-heder' : this._stikyHeader,
				'vue-table_selectable-rows': this._selectableRows,
				tableClass: this.tableClass
			};
			if (this.tableClass)
			{
				cl[this.tableClass] = true;
			}
			return cl;
		},

		/**
		 * Заголовки таблицы
		 */
		_tableHeaders(): ITableHeadCol[][]
		{
			return this.tableHeaders ?? [];
		},
		/**
		 * данные таблицы
		 */
		// _tableData(): any[]
		// {
		// 	return this.tableData ?? [];
		// },
		/**
		 * поля таблицы
		 */
		// _tableFields(): string[]
		// {
		// 	return this.tableFields ?? [];
		// },
		/**
		 * Закрепить заголовок
		 */
		_stikyHeader(): boolean
		{
			return this.stikyHeader;
		},
		/**
		 * Количество закрепленных столбцов
		 */
		_stikyColumns(): number
		{
			return this.stikyColumns
		},
		/**
		 * Выделение столбцов
		 */
		_selectableColumns(): boolean
		{
			return this.selectableColumns;
		},
		/**
		 * выделение строк
		 */
		_selectableRows(): boolean
		{
			return this.selectableRows;
		},
	},

	methods: {
		moveByTable(e: MouseEvent)
		{
			let target = (e.target as HTMLElement).closest('td,th') as HTMLElement;
			if (target)
			{
				this.currentIndex = (target as HTMLElement).dataset.ceilIndex
					? Number((target as HTMLElement).dataset.ceilIndex as string)
					: null;
			}
		},
		moveOutFromTable(/*e: MouseEvent*/)
		{
			//let target = e.target ? e.target.closest('td,th') as HTMLElement : null;
			//let related = e.relatedTarget ? e.relatedTarget.closest('td,th') as HTMLElement : null;
			//console.log(target, related, target == related)
			//if (target != related)
				this.currentIndex = null;
		},
		setStickyCols(numCols: number)
		{
			let colunmSizes: number[] = [0]; //Хранит размеры всех колонок для рассчета суммарных отступов
			
			for(let n = 0; n < numCols; n++)
			{	
				let th: HTMLElement = (this.$refs.mainTable as HTMLElement).querySelector(`.vue-table_head>tr>th:nth-child(${n+1})`)!;
				if (th)
				{
					let size = (th as HTMLElement).getAttribute('width') ?? Math.ceil(th.offsetWidth);
					//console.log(th.offsetWidth, th.width);
					colunmSizes.push(Number(size));
					//суммарный отступ для каждого последующего элемента
					let offset = colunmSizes.slice(0, n+1).reduce((sum, elem) => sum + elem, 0);
					
					th.style.width = String(size) + 'px';
					th.style.left = String(offset ?? 0) + 'px';
					th.classList.add('sticked');

					let td_s: NodeListOf<HTMLElement> = (this.$refs.mainTable as HTMLElement).querySelectorAll(`.vue-table_body>tr>td:nth-child(${n+1})`);
					td_s.forEach((el: HTMLElement) => {
						(el as HTMLElement).style.width = String(size) + 'px';
						(el as HTMLElement).style.left = String(offset) + 'px';
						(el as HTMLElement).classList.add('sticked');
					});
				}
			}
		},
	},
	mounted()
	{
		if (this._stikyColumns > 0)
		{
			this.setStickyCols(this._stikyColumns);
		}

		if (this._selectableColumns)
		{
			(this.$refs.mainTable as HTMLElement).addEventListener('mouseover', this.moveByTable);
			(this.$refs.mainTable as HTMLElement).addEventListener('mouseout', this.moveOutFromTable);
		}
	},
	beforeUnmount() {
		if (this._selectableColumns)
		{
			(this.$refs.mainTable as HTMLElement).removeEventListener('mouseover', this.moveByTable);
			(this.$refs.mainTable as HTMLElement).removeEventListener('mouseout', this.moveOutFromTable);
		}
	},
	updated() {
		if (this._stikyColumns > 0)
			{
				this.setStickyCols(this._stikyColumns);
			}
	},
})
export default VueTableWrapper;
export type IVueTableWrapper = typeof VueTableWrapper;
export interface ITableHeadCol{
	colunm: string,
	label: string,
}
</script>
<style lang="scss">

.vue-table {
	tbody {
		td.vue-table-ceil_selected {
			background-color: rgb(230, 238, 246) !important;
		}
	}
	td.sticked, th.sticked{
		position: sticky;
		background-color: white;
		z-index: 1;
	}
}

.vue-table.vue-table_sticky-heder{
	border-collapse: separate;
    border-spacing: 0;
	thead.vue-table_head{
		tr{
			position: sticky;
			top:0;
			z-index: 10;
		}
	}
}

.vue-table.vue-table_selectable-rows{
	tbody.vue-table_body > {
		tr:hover:not(.additionally-row) > {
			td{
				background-color: rgb(230, 238, 246) !important;
			}
		}
	}
}

.vue-table_head{
	//Заголовок
}
.vue-table_body{
	//тело
}


</style>