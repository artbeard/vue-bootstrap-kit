<template>
	<div class="dropdown" :class="className">
		<div @click="toggleMenu">
			<slot name="toggler">
				<button class="btn btn-default">
					Открыть
				</button>
			</slot>
		</div>
		<DropDown ref="menu_dropDownMenu">
			<slot name="menu"></slot>
		</DropDown>
	</div>
</template>
<script lang="ts">
import { defineComponent } from 'vue'
import DropDown from "./DropDown.vue";

export default defineComponent({
	name: 'DropDownMenu',
	components: {
		DropDown
	},
	data() {
		return {
			//menu_dropDownMenu: null  as null | typeof DropDown
			//dropdown-menu-start|end dropdown-menu-arrow bg-dark text-white
		}
	},
	props:
	{
		// mode: {
		//     type: String,
		//     default: 'normal'
		// }
		className: {
			type: String,
			default: ''
		}
	},
	methods: {
		close()
		{
			(this.$refs.menu_dropDownMenu as typeof DropDown).hide();
			document.removeEventListener('click', this.onOpenedMenu);
		},
		onOpenedMenu(/*e: Event*/){
			this.close();
		},

		open(event: Event)
		{
			(this.$refs.menu_dropDownMenu as typeof DropDown).toggle(event);
			setTimeout(()=>{
				document.addEventListener('click', this.onOpenedMenu);
			})
		},
		toggleMenu(event: Event)
		{
			if ((this.$refs.menu_dropDownMenu as typeof DropDown).getOpenedStatus())
			{
				this.close();
			}
			else
			{
				this.open(event);
			}
		}
	},

})
</script>
