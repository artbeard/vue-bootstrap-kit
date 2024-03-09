<template>
	<div class="card">
		<div class="card-header">
			<ul class="nav nav-tabs card-header-tabs" data-bs-toggle="tabs" role="tablist">
				<li class="nav-item" role="presentation"
				    v-for="prp in tabTitles"
				    @click="selectedTitle = prp.title"
				    :key="prp.title"
				    >
					<a href="javascript:void(0);" class="nav-link" :class="{ active: selectedTitle === prp.title }" data-bs-toggle="tab" aria-selected="true" role="tab">
						<i v-if="prp.icon" class="icon me-2" :class="prp.icon"></i>
						{{ prp.title }}
					</a>
				</li>
			</ul>
		</div>
		<div class="card-body">
			<slot></slot>
		</div>
	</div>
</template>
<script lang="ts">
import { defineComponent } from "vue";
export default defineComponent({});
</script>
<script setup lang="ts">
import { useSlots, ref, provide } from "vue";
const slots = useSlots();
const tabTitles = ref<any[]>((slots.default!() as Array<any>).map((tab) => ({
	title: tab.props?.title,
	icon: tab.props?.icon ?? null
})));
const selectedTitle = ref(tabTitles.value[0].title);
provide('selectedTitle', selectedTitle);
</script>
