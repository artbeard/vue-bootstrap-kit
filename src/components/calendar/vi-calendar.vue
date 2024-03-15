<template>
<!--	<pre>{{dateMatrix.length}},{{dateMatrix}}, {{ dateMatrix[35] }}</pre>-->
<!--	<div>-->
<!--		<span class="px-1" v-for="day in dateMatrix">{{day.getDate()}}</span>-->
<!--	</div>-->
	<div class="vi-calendar">
		<div class="vi-weak-control">
			<div class="vi-day bg-light"><strong>пн</strong></div>
			<div class="vi-day bg-light"><strong>вт</strong></div>
			<div class="vi-day bg-light"><strong>ср</strong></div>
			<div class="vi-day bg-light"><strong>чт</strong></div>
			<div class="vi-day bg-light"><strong>пт</strong></div>
			<div class="vi-day bg-light text-red"><strong>сб</strong></div>
			<div class="vi-day bg-light text-red"><strong>вс</strong></div>
		</div>

		<template v-for="i in (dateMatrix.length/7 + 1)">
			<div class="vi-weak">
				<div class="vi-day"
				     v-for="(day, index) in dateMatrix.slice((i-1) * 7, (i-1) * 7 + 7)"
				     :key="`${day.getDate()}_${day.getMonth()}`"
				     :class="{
						 'bg-light': isCurrentMonth(day),
						 'border-azure': isCurrentDay(day)
				     }"
				>
					<div class="vi-day_num">
						<div class="badge"
						     :class="{ 'bg-red-lt': isWeekEnd(day), 'bg-dark-lt': !isWeekEnd(day)}"
						>
							{{day.getDate()}}
						</div>
					</div>
					<div class="vi-day_content ">
						<template v-if="isCurrentDay(day)">
							<div style="max-width: 100%; overflow: hidden">
								<div class="badge badge-outline text-indigo ">
									Некоторое содержание длинный текст
								</div>
								<div class="badge badge-outline text-orange ">
									Некоторое
								</div>
								<div class="badge badge-outline text-green ">
									Некоторое содержание длинный текст
								</div>
							</div>

						</template>


						<div style=" display: none;
						/*показывать это, когда есть переполнение*/
    background-image: linear-gradient(#ffffff82, #ffffffd9, #ffffff);
    min-height: 25px;
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
"></div>
					</div>
				</div>
			</div>
		</template>
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
		},
		isCurrentMonth()
		{
			return (testDate: Date): boolean => testDate.getMonth() == this.getDisplayDate.getMonth();
		},
		isCurrentDay()
		{
			return (testDate: Date): boolean => {
				const curDate = new Date();
				return testDate.getFullYear() == curDate.getFullYear() &&
					testDate.getMonth() == curDate.getMonth() &&
					testDate.getDate() == curDate.getDate();
			};
		},
		isWeekEnd()
		{
			return (testDate: Date): boolean => [0, 6].indexOf(testDate.getDay()) > -1;
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

	beforeMount() {
		this.dateMatrix = [...this.generateNewMatrix(this.getDisplayDate)];
	}

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
			text-transform: capitalize;
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
				//display: grid;
				text-align: justify;
				max-height: 15vh;
				overflow: hidden;
				overflow-y: scroll;
				-ms-overflow-style: none;  /* IE и Edge */
				scrollbar-width: none;  /* Firefox */

				div{
					max-width: 100%;
					overflow: hidden;
					text-overflow: ellipsis;
				}
			}

			.vi-day_content::-webkit-scrollbar {
				display: none;
			}
		}
	}


}
</style>