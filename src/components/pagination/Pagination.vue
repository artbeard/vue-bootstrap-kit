<script lang="ts">
import {defineComponent} from 'vue'

interface IPages{
	pages: number[],
	edgeLinks: (number|null)[]
}

export default defineComponent({
	name: 'Pagination',
	props: {
		total: {
			type: Number,
			default: 0
		},
		pageLength: {
			type: Number,
			default: 10
		},
		page: {
			type: Number
		},
		countLink: {
			type: Number,
			default: 5
		},
	},
	emits: ['update:page', 'goToPage'],
	computed: {
		_page: {
			get(): number
			{
				return this.page ?? 0
			},
			set(newPage: number)
			{
				this.$emit('update:page', newPage);
			}
		},
		_pageLength(): number
		{
			return this.pageLength;
		},
		_countLink(): number
		{
			return this.countLink;
		},
		_total(): number
		{
			return this.total ?? 0;
		},
		isActive(): (index: number) => boolean
		{
			return (index: number): boolean => this._page == index;
		},

		countPage(): number
		{
			return Math.ceil(this._total / this._pageLength);
		},
		generatePageList(): IPages
		{
			let pages: number[] = [];
			let edgeLinks: (number | null)[] = [];

			if (this.countPage <= this._countLink)
			{
				//генерация без концевых кнопок
				for (let i:number = 0; i < this.countPage; i++)
				{
					pages.push(i);
				}

				//console.log('МИН', 'Страниц', this.countPage, 'Ссылок', this._countLink, 'Записей', this._total);
			}
			else //if (this._page < Math.round(this._countLink / 2))
			{
				let min: number = this._page - Math.floor(this._countLink / 2);
				let max: number = this._page + Math.round(this._countLink / 2);
				edgeLinks[0] = 0;
				edgeLinks[1] = this.countPage;

				//console.log(min , max, 'Страниц', this.countPage, 'Ссылок', this._countLink, 'Записей', this._total);

				//Коррекция фрейма при выходе за границы
				if (min < 0)
				{
					edgeLinks[0] = null;
					min = 0;
					max = min + this._countLink;
				}
				if (max > this.countPage)
				{
					edgeLinks[1] = null;
					max = this.countPage;
					min = max - this._countLink;
				}
				//console.log('Пересчет', min , max, 'Страниц', this.countPage, 'Ссылок', this._countLink, 'Записей', this._total);
				//сокрытие граничных переходов если фрейм находится у границы
				if (min == 0)
				{
					edgeLinks[0] = null;
				}
				if (max == this.countPage)
				{
					edgeLinks[1] = null;
				}

				for (let i:number = min; i < max; i++)
				{
					pages.push(i);
				}
			}

			return {
				pages,
				edgeLinks
			}
		}
	},
	methods: {
		setPage(newPage: number)
		{
			setTimeout(() => {
				this.$emit('goToPage', newPage);
				this._page = newPage;
			})
		},
	}
})

</script>

<template>
	<ul class="pagination ">
		<li class="page-item _disabled" v-if="generatePageList.edgeLinks[0] !== undefined && generatePageList.edgeLinks[0] !== null">
			<a class="page-link" href="javascript:void(0);" @click="setPage(generatePageList.edgeLinks[0])" aria-disabled="true">
				<svg xmlns="http://www.w3.org/2000/svg" class="icon" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round"><path stroke="none" d="M0 0h24v24H0z" fill="none"></path><path d="M15 6l-6 6l6 6"></path></svg>
			</a>
		</li>

		<li class="page-item" v-for="page in generatePageList.pages" :key="page">
			<template v-if="isActive(page)">
				<a class="page-link" href="javascript:void(0);" :class="'active'" >{{page + 1}}</a>
			</template>
			<template v-else>
				<a class="page-link" href="javascript:void(0);" @click="setPage(page)" >{{page + 1}}</a>
			</template>

		</li>

		<!-- li class="page-item active"><a class="page-link" href="#">2</a></li>
		<li class="page-item"><a class="page-link" href="#">3</a></li>
		<li class="page-item"><a class="page-link" href="#">4</a></li>
		<li class="page-item"><a class="page-link" href="#">5</a></li -->

		<li class="page-item" v-if="generatePageList.edgeLinks[1] !== undefined && generatePageList.edgeLinks[1] !== null">
			<a class="page-link" href="javascript:void(0);" @click="setPage(generatePageList.edgeLinks[1])">
				<svg xmlns="http://www.w3.org/2000/svg" class="icon" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round"><path stroke="none" d="M0 0h24v24H0z" fill="none"></path><path d="M9 6l6 6l-6 6"></path></svg>
			</a>
		</li>
	</ul>
</template>

<style scoped>

</style>