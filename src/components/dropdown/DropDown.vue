<template>
	<div class="dropdown-menu show"
	     ref="dropDown"
	     v-show="shown"
	     :class="[
			arrow == 'left' ? 'dropdown-menu-arrow' : (arrow == 'right' ? 'dropdown-menu-arrow dropdown-menu-end' : '')
		]"
	     :data-dropdown-uuid="uuid"
	     data-bs-popper="static">
		<span v-if="title != null" class="dropdown-header">{{ title }}</span>
		<slot></slot>
	</div>
</template>
<script lang="ts">
import {defineComponent, nextTick, type PropType} from 'vue'
export default defineComponent({
    name: 'DropDown',
    data() {
        return {
            target: null as null | HTMLElement,
            shown: true as boolean,
            uuid: '' as string,
            //dropdown-menu-start|end dropdown-menu-arrow bg-dark text-white
        }
    },
    props:{
	    mode: {
		    type: String,
		    default: 'normal'
	    },
	    horizontalAlign: {
		    type: String,
		    default: null
	    },
	    arrow: {
		    type: String, //right, left
		    default: 'right'
	    },
	    title: {
		    type: String as PropType<string | null>,
		    default: null
	    },
    },
    computed:{

    },
    methods: {
	    show(event: Event) {
		    setTimeout(() => {

			    this.target = event.target as HTMLElement;
			    this.shown = true;
			    if (this.mode === 'float') {
				    let targetSize = (this.target as HTMLElement).getBoundingClientRect();
				    let el: HTMLElement = this.$refs.dropDown as HTMLElement;
				    //let elSize = el.getBoundingClientRect()
				    //console.log(box)
				    if (!this.uuid) {
					    //el.style.width = elSize.width + 'px';
					    if (this.arrow == 'right') {
						    el.style.right = '10px';
					    }
					    else {
						    el.style.left = '10px';
					    }
					    el.style.top = '10px';

					    this.uuid = this.uustr();

					    nextTick(() => {
						    targetSize = (this.target as HTMLElement).getBoundingClientRect();
						    //elSize = el.getBoundingClientRect()
						    //el.style.width = elSize.width + 'px';
						    //el.style.top = Math.round(elSize.height / 2 + targetSize.top) + 'px';
						    el.style.top = Math.round(targetSize.top + targetSize.height / 2) + 'px';
						    if (this.arrow == 'right') {
							    //console.log(this.target, targetSize, elSize)
							    el.style.right = Math.round(document.documentElement.clientWidth - targetSize.right) + 'px';
						    }
						    else {
							    el.style.left = Math.round(targetSize.left - targetSize.width / 2 + 14 * 2) + 'px'; //14px - поправка на отступ для стрелки
						    }


					    })
				    }
			    }

		    })
	    },
	    hide() {
		    this.target = null;
		    this.shown = false;

	    },
	    toggle(event) {
		    this.target = event.target as HTMLElement;
		    this.shown = !this.shown;
	    },
	    getOpenedStatus(): boolean {
		    return this.shown
	    },

	    closeEvent(event) {
		    let target = (event.target as HTMLElement).closest(`[data-dropdown-uuid="${this.uuid}"]`);
		    //console.log('closeEvent')
		    if (!target && this.shown) {
			    //console.log('closeEvent hide')
			    this.hide()
			    this.uuid = '';
		    }
	    },

	    uustr(i = 12) {
		    let rnd = '';
		    while (rnd.length < i)
			    rnd += Math.random().toString(36).substring(2);
		    return rnd.substring(0, i);
	    }
    },
	mounted() {
		this.hide()
		if (this.mode === 'float') {
			//console.log('set remove event listenner')
			setTimeout(() => {
				document.body.addEventListener('click', this.closeEvent.bind(this))
			})
		}
	},
	unmounted() {
		setTimeout(() => {
			document.body.removeEventListener('click', this.closeEvent.bind(this))
		})
	},
	updated() {
		//console.log('updated')
	},
})
</script>
<style>
.dropdown-item{
    cursor:pointer;
}
</style>