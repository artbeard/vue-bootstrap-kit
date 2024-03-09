<template>
	<div class="popover fade shadow-lg" role="tooltip"
		:class="[direction == 'ltr' ? 'bs-popover-end' : 'bs-popover-start', visibility ? 'show' : '' ]"
		v-show="shown"
		ref="popOver"
		style="position: absolute; margin: 0px;"
		data-popper-placement="right">
		<div class="popover-arrow" ref="popOverArrow" style="position: absolute; top: 0px;"></div>
		<!-- <h3 class="popover-header">Popover title</h3> -->
		<div class="popover-body">
			<slot></slot>
		</div>
	</div>
</template>
<script lang="ts">
import { defineComponent } from 'vue'
export default defineComponent({
	name: 'Popover',
	data() {
		return {
			direction: 'ltr' as string,
			shown: false as boolean,
			visibility: false as boolean,
			target: {} as HTMLElement,
		}
	},
	methods:{
		toggle (event: MouseEvent){
			console.log(event)
		},
		show(e: MouseEvent){
			let popover: HTMLElement = <HTMLElement>this.$refs.popOver;
			popover.style.transform = ``;
			let target: HTMLElement = <HTMLElement>e.target;
			this.direction = (document.documentElement.clientWidth - e.clientX < document.documentElement.clientWidth / 4) ? 'rtl' : 'ltr';
			this.target = target;
			this.shown = true;

			setTimeout(() => {
				let targetSize = (target as HTMLElement).getBoundingClientRect();
				let popoverSize = (this.$refs.popOver as HTMLElement).getBoundingClientRect();
				let popoverAbsSize = this.getBoundingDocumentRect(this.$refs.popOver as HTMLElement);

				let dY1 = popoverSize.top - targetSize.top - targetSize.height; //поправка на относительные блоки внутри страницы

				let dX = popoverSize.x - targetSize.x;

				let dY = window.scrollY + dY1;
				let left = this.direction == 'rtl'
					? -1 * popoverSize.width
					: Math.round(targetSize.width / 2);
				left = left - dX;


				let middleH = Math.round(popoverAbsSize.height / 2); //середина плашки
				let deltaH = Math.round(middleH + (targetSize.height / 2));
				let angleH = Math.round(middleH - 16/2); //высота уголка(стрелки)
				let top = popoverAbsSize.top - popoverSize.top - deltaH;

				//краевая поправка, когда плашка около низа / верха окна
				let bottomAmendment = popoverSize.height - (document.documentElement.clientHeight - (targetSize.top + targetSize.height));
				let topAmendment = popoverSize.height - targetSize.top;

				if (bottomAmendment > 0)
				{
					dY = dY + bottomAmendment / 2
					angleH = angleH + bottomAmendment / 2
				}
				else if (topAmendment > 0)
				{
					dY = dY - topAmendment / 2
					angleH = angleH - topAmendment / 2
				}

				top = top - dY;
				popover.style.transform = `translate(${left}px, ${top}px)`;
				let popOverArrow: HTMLElement = <HTMLElement>this.$refs.popOverArrow;
				popOverArrow.style.transform = `translate(0px, ${angleH}px)`;
				this.visibility = true;
			})
		},
		hide()
		{
			if (this.$refs.popOver instanceof HTMLElement)
				this.$refs.popOver.style.transform = ``;
			this.visibility = false;
			this.shown = false;
		},

		getBoundingDocumentRect(elem: HTMLElement)
		{
			let box = elem.getBoundingClientRect();
			return {
				height:  box.height,
				width:   box.width,
				top:     box.top + window.pageYOffset,
				right:   box.right + window.pageXOffset,
				bottom:  box.bottom + window.pageYOffset,
				left:    box.left + window.pageXOffset
			}
		}
	},
	mounted(){
		// const ro = new ResizeObserver(entries => {
		// 	for(let entry of entries){
		//
		// 		let tblSize = (document.querySelector('.realtime_table') as HTMLElement).getBoundingClientRect()
		// 		const dTop = tblSize.top; //Этот костыль нужен для коррекции всплывашки по высоте
		// 		//const targetSize = ((this.target as HTMLElement).closest('td') as HTMLElement).getBoundingClientRect();
		// 		const targetSize = (this.target as HTMLElement).getBoundingClientRect();
		// 		let elemSize = (entry.target as HTMLElement).getBoundingClientRect();
		// 		(entry.target as HTMLElement).style.top = `${targetSize.top}px`;
		// 		(entry.target as HTMLElement).style.left = `${targetSize.left}px`;
		//
		// 		let left = this.direction == 'ltr' ? targetSize.width/2 : -targetSize.width / 3 - entry.contentRect.width ;
		//
		// 		(entry.target as HTMLElement).style.transform = `translate(${left}px, ${ (- entry.contentRect.height / 2) - dTop + targetSize.height}px)`;
		// 		(this.$refs.popOverArrow as HTMLElement).style.transform = `translate(0px, ${elemSize.height / 2 - targetSize.height}px)`;
		// 	}
		// })
		// ro.observe((this.$refs.popOver as HTMLElement))
	},
	beforeUnmount() {
		
	},
})
</script>
