<template>
	<div class="tab-content" v-show="selectedTitle === title">
		<slot></slot>
	</div>
</template>
<script lang="ts">
import { defineComponent } from 'vue'
export default defineComponent({

})
</script>
<script setup lang="ts">
import { inject } from 'vue'
//@ts-ignore
const props = defineProps({
	title: String,
	icon: String
})
const selectedTitle = inject('selectedTitle')
</script>
