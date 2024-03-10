<template>
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
		<div class="vi-weak">
			<div class="vi-day">
				<div class="vi-day_num">26</div>
				<div class="vi-day_content">
				</div>
			</div>
			<div class="vi-day">
				<div class="vi-day_num">27</div>
				<div class="vi-day_content"></div>
			</div>
			<div class="vi-day">
				<div class="vi-day_num">28</div>
				<div class="vi-day_content"></div>
			</div>
			<div class="vi-day">
				<div class="vi-day_num">29</div>
				<div class="vi-day_content"></div>
			</div>
			<div class="vi-day">
				<div class="vi-day_num">30</div>
				<div class="vi-day_content"></div>
			</div>
			<div class="vi-day">
				<div class="vi-day_num">31</div>
				<div class="vi-day_content"></div>
			</div>
			<div class="vi-day">
				<div class="vi-day_num">1</div>
				<div class="vi-day_content"></div>
			</div>
		</div>
		<div class="vi-weak">
			<div class="vi-day">
				<div class="vi-day_num">26</div>
				<div class="vi-day_content">
				</div>
			</div>
			<div class="vi-day">
				<div class="vi-day_num">27</div>
				<div class="vi-day_content"></div>
			</div>
			<div class="vi-day">
				<div class="vi-day_num">28</div>
				<div class="vi-day_content"></div>
			</div>
			<div class="vi-day">
				<div class="vi-day_num">29</div>
				<div class="vi-day_content"></div>
			</div>
			<div class="vi-day">
				<div class="vi-day_num">30</div>
				<div class="vi-day_content"></div>
			</div>
			<div class="vi-day">
				<div class="vi-day_num">31</div>
				<div class="vi-day_content"></div>
			</div>
			<div class="vi-day">
				<div class="vi-day_num">1</div>
				<div class="vi-day_content"></div>
			</div>
		</div>
	</div>
</template>

<script lang="ts">
import {defineComponent} from 'vue'

export default defineComponent({
	name: 'vi-calendar'
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