<template>
	<pre>{{dateMatrix.length}},{{dateMatrix}}, {{ dateMatrix[35] }}</pre>
	<div>
		<span class="px-1" v-for="day in dateMatrix">{{day.getDate()}}</span>
	</div>
	<div class="vi-calendar">
		<div class="vi-weak-control">
			<div class="vi-day">пн</div>
			<div class="vi-day">вт</div>
			<div class="vi-day">ср</div>
			<div class="vi-day">чт</div>
			<div class="vi-day">пт</div>
			<div class="vi-day">сб</div>
			<div class="vi-day">ВС</div>
		</div>

		<template v-for="i in (dateMatrix.length/7 + 1)">
			<div class="vi-weak">
				<div class="vi-day" v-for="(day, index) in dateMatrix.slice((i-1) * 7, (i-1) * 7 + 7)" :key="`${day.getDate()}_${day.getMonth()}`">
					<div class="vi-day_num">{{day.getDate()}}</div>
					<div class="vi-day_content">
						{{index}},
						{{ (i - 1) * 7 }} - {{ (i - 1) * 7 + 7 }}

					</div>
				</div>
			</div>
		</template>

<!--		&#45;&#45;-->
<!--		<div class="vi-weak">-->
<!--			<div class="vi-day">-->
<!--				<div class="vi-day_num">26</div>-->
<!--				<div class="vi-day_content">-->
<!--				</div>-->
<!--			</div>-->
<!--			<div class="vi-day">-->
<!--				<div class="vi-day_num">27</div>-->
<!--				<div class="vi-day_content"></div>-->
<!--			</div>-->
<!--			<div class="vi-day">-->
<!--				<div class="vi-day_num">28</div>-->
<!--				<div class="vi-day_content"></div>-->
<!--			</div>-->
<!--			<div class="vi-day">-->
<!--				<div class="vi-day_num">29</div>-->
<!--				<div class="vi-day_content"></div>-->
<!--			</div>-->
<!--			<div class="vi-day">-->
<!--				<div class="vi-day_num">30</div>-->
<!--				<div class="vi-day_content"></div>-->
<!--			</div>-->
<!--			<div class="vi-day">-->
<!--				<div class="vi-day_num">31</div>-->
<!--				<div class="vi-day_content"></div>-->
<!--			</div>-->
<!--			<div class="vi-day">-->
<!--				<div class="vi-day_num">1</div>-->
<!--				<div class="vi-day_content"></div>-->
<!--			</div>-->
<!--		</div>-->

	</div>

</template>

<script lang="ts">
import {defineComponent} from 'vue';
import type { PropType } from 'vue';
export default defineComponent({
	name: 'vi-calendar',

	data(){ return {

		dateMatrix: [],

	}},

	props: {
		date: {
			type: [String, Date],// Object as PropType<Date | string>
			default(){
				return new Date();
			}
		},
	},

	watch: {
		getDisplayDate(newVal, oldVal)
		{
			// console.log('Пересчитываем матрицу дат для ', newVal);
			// const firstDate = new Date(newVal.getFullYear(), newVal.getMonth(), 1);
			// let rewindDays = (firstDate.getDay() == 0 ? 7 : firstDate.getDay()) - 1;
			// let countDays = 33 - new Date(firstDate.getFullYear(), firstDate.getMonth(), 33).getDate()
			// let countDisplayDays = Math.ceil((rewindDays + countDays) / 7);
			//
			//
			// firstDate.setDate(firstDate.getDate() - rewindDays);
			// console.log(firstDate.toLocaleDateString(), firstDate.getDay(),
			// 	countDays,
			// 	countDisplayDays
			// )
			this.dateMatrix = [...this.generateNewMatrix(newVal)];
		}
	},

	computed: {
		getDisplayDate(): Date
		{
			if (typeof this.date === 'string')
			{
				let _date = new Date(this.date);
				if (isNaN(_date.valueOf()))
				{
					console.error('Date prop is invalid: ', this.date);
					return new Date();
				}
				return _date;
			}
			else
			{
				return <Date>this.date;
			}
		}
	},

	methods: {
		generateNewMatrix(date: Date)
		{
			const firstDate = new Date(date.getFullYear(), date.getMonth(), 1);
			let rewindDays = (firstDate.getDay() == 0 ? 7 : firstDate.getDay()) - 1;
			let countDays = 33 - new Date(firstDate.getFullYear(), firstDate.getMonth(), 33).getDate()
			let countDisplayDays = Math.ceil((rewindDays + countDays) / 7) * 7;

			firstDate.setDate(firstDate.getDate() - rewindDays);
			function * matrixGenerator(start, count)
			{
				for (let i = 0; i < count; i++)
				{
					start.setDate(start.getDate() + (!!i ? 1 : 0));
					yield new Date(start.getTime())
				}
			}
			return matrixGenerator(firstDate, countDisplayDays);
		},
	},

})
</script>


<style scoped lang="scss">
.vi-calendar{
	height: calc( 100vh - 180px );
	display: flex;
	flex-direction: column;
	.vi-weak-control{
		flex-direction: row;
		display: flex;
		//flex: auto;
		flex-shrink: 2;
		.vi-day{
			border-radius: 5px;
			border: 1px solid rgba(50,50,50, 0.1);
			flex: 1;
			padding: 0.125em;
		}
	}
	.vi-weak{
		flex-direction: row;
		display: flex;
		flex: auto;
		flex-shrink: 1;
		.vi-day{
			//height: calc( (100vh - 70px) / 7 );
			border-radius: 5px;
			border: 1px solid rgba(50,50,50, 0.1);
			flex: 1;
			padding: 0.125em;
			position: relative;
			//flex-shrink: 1;
			//flex-grow: 1;
			.vi-day_num{
				font-weight: 600;
				position: absolute;
				right: 5px;
				top: 5px;
			}

			.vi-day_content{
				text-align: justify;
			}
		}
	}


}
</style>