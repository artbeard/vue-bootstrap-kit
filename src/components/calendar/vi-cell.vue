<template>
	<div class="vi-day_content" ref="dayContent" @scroll="scroll">
		<div class="vi-day-content_inner" ref="dayContentInner">
			<slot></slot>
		</div>
	</div>
	<div style=" _display: none;
			/*показывать это, когда есть переполнение*/
			    background-image: linear-gradient(#ffffff82, #ffffffd9, #ffffff);
			    min-height: 25px;
			    position: absolute;
			    left: 0;
			    right: 0;
			    bottom: 0;
			"></div>
</template>

<script lang="ts">
import {defineComponent} from 'vue'

export default defineComponent({
	name: "vi-cell",
  methods: {
    scroll(){
      console.log(
          (this.$refs.dayContent as HTMLElement).offsetHeight,
          (this.$refs.dayContentInner as HTMLElement).offsetHeight
      )
    }
  }
})
</script>


<style scoped lang="scss">
.vi-calendar{
	.vi-weak{
		.vi-day{
			.vi-day_content{
				//display: grid;
				text-align: justify;
				max-height: 15vh;
				overflow: hidden;
				overflow-y: scroll;
				-ms-overflow-style: none;  /* IE и Edge */
				scrollbar-width: none;  /* Firefox */
				position: absolute;
				left: 0;
				right: 0;
				bottom: 0;
				top: 0px;

				.vi-day-content_inner{
					max-width: 100%;
					overflow: hidden
				}

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