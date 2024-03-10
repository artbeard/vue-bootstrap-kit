<script setup lang="ts">
import {ref, reactive} from "vue";

//import Button from "./components/button/Button.vue";
import SlimSelect, {type ISelectOptionInterface} from "./components/select/SlimSelect.vue";
import DropDownMenu from "./components/dropdown/DropDownMenu.vue";
import Popover from "./components/popover/Popover.vue";
import Modal from "./components/modal/Modal.vue";
import Jodit from "./components/wysiwyg/jodit.vue";
import Pagination from "./components/pagination/Pagination.vue";
import TabPanel from "./components/tab/TabPanel.vue";
import Tab from "./components/tab/Tab.vue";
import Flatpickr from "./components/datepicker/Flatpickr.vue";
import Offcanvas from "./components/offcanvas/Offcanvas.vue";
import viCalendar from "./components/calendar/vi-calendar.vue";




const popoverA = ref<typeof Popover>();
function popOpen(e: MouseEvent)
{
	(popoverA.value as typeof Popover).show(e);
}
function popHide(e: MouseEvent)
{
	(popoverA.value as typeof Popover).hide(e);
}

const modal1 = ref<typeof Modal>()
function howModal()
{
	(modal1.value as typeof Modal).show();
}


const joditText = ref<string>('начальное значение')

const pagPage = ref<number>(1)
const pagPageLength = ref<number>(5)
const pagTotal = ref<number>(507)

const dateFlatPicr = ref<string>('');


const slimSelectVal1 = ref<string>('');
const slimSelectVal2 = ref<string[]>([]);
const slimSelectValDynamic = ref<string[]>(['']);
const optionList = reactive<ISelectOptionInterface[]>([
	{ value: '', text: 'Плейсхолдер', placeholder: true },
	{ value: 'option 1', text: 'Опция 1' },
	{ value: 'option 2', text: 'Опция 2' },
	{ value: 'option 3', text: 'Опция 3' },
])
const addToOptList = function ()
{
	let time = new Date();
	for (let i = 0; i < 5; i++)
	{
		optionList.push({value: `option ${time.getTime()}${i}`, text: `Опция ${time.getTime()}${i}`})
	}
}

const clearSelected = function ()
{
	slimSelectVal1.value = '';
	slimSelectVal2.value = [];
}

</script>

<template>
	<header class="navbar navbar-expand-md navbar-light d-print-none">
		<div class="container-xl">

			<button class="navbar-toggler collapsed" type="button">
				<span class="navbar-toggler-icon"></span>
			</button>

			<!-- Логотип -->
			<h1 class="navbar-brand navbar-brand-autodark d-none-navbar-horizontal pe-0 pe-md-3">
				<a href="/">
					Логотип
				</a>
			</h1>

			<!-- Выпаадающее меню профиля -->
			<div class="navbar-nav flex-row order-md-last">
			</div>

			<div class="collapse navbar-collapse show" id="navbar-menu">
				<div class="d-flex flex-column flex-md-row flex-fill align-items-stretch align-items-md-center">
					<ul class="navbar-nav">
					</ul>
				</div>
			</div>
		</div>
	</header>

	<!-- Navbar -->

	<div class="page-wrapper">
		<!-- <router-view></router-view> -->

		<div class="page-header d-print-none">
			<div class="container-xl">
				<div class="row align-items-center">
					<div class="col">
						<div class="mb-1"></div>
						<h2 class="page-title">Компоненты</h2>
					</div>
					<div class="col-auto ms-auto"></div>
				</div>
			</div>
		</div>

		<div class="page-body">
			<div class="container-xl">
				<div class="row row-cards mb-3">
					<div class="col-3">
						<div class="card">
							<div class="card-body">управление</div>
						</div>

					</div>
					<div class="col-9">
						<vi-calendar></vi-calendar>
					</div>
				</div>
				<div class="row row-cards">


					<div class="card">
						<div class="card-body">
							<div class="row">
								<div class="col-6">
									<SlimSelect class="mb-3" v-model="slimSelectVal1" :options="optionList" />
									<p class="mb-3">Выбранное значение</p>
									<pre>{{ slimSelectVal1 }}</pre>
									<SlimSelect class="mb-3" v-model="slimSelectVal2" :options="optionList" :multiple="true" :close-on-select="false" />
									<p class="mb-3">Выбранное значение</p>
									<pre>{{ slimSelectVal2 }}</pre>
									<button class="btn btm-sm" @click="addToOptList">
										ch
									</button>
									<button class="btn btm-sm" @click="clearSelected">
										clear
									</button>

									<div v-for="(ssel_d, index) in slimSelectValDynamic" :key="index" >
										<SlimSelect class="mb-3" v-model="slimSelectValDynamic[index]" :options="optionList" />
									</div>
									<p class="mb-3">Выбранное значение</p>
									<pre>{{ slimSelectValDynamic }}</pre>

								</div>

								<div class="col-6">
									<p>Компонент select с возможностью поиска и множественным выбором</p>
								</div>
							</div>
						</div>
					</div>



					<div class="col-1">
						<div class="card">
							<DropDownMenu class="abc">
								<template #menu>
									<span class="dropdown-item">123456</span>
									<span class="dropdown-item">98745</span>
								</template>
							</DropDownMenu>
						</div>
					</div>

					<div class="col-1">
						<div class="card">
							<div @mouseover="popOpen" @mouseleave="popHide" >наведи</div>
							<Popover ref="popoverA">
								Всплывашка!
							</Popover>
						</div>
					</div>

					<div class="col-1">
						<div class="card">
							<button class="btn btn-secondary" @click="howModal">Модалка</button>
							<Modal ref="modal1" pad="m-0" :ok-button="`ok button`">
								Содержимое модального окна!
							</Modal>
						</div>
					</div>
					<div class="col-12 col-lg-6">
						<div class="card">
							<div class="card-body">
								<Jodit v-model="joditText" />
							</div>
							<div class="card-body">
								<button class="btn btn-sm" @click="joditText = 'cleared!'">
									clear
								</button>
								<pre>{{joditText}}</pre>
							</div>
						</div>
					</div>

					<div class="col-12 col-lg-6">
						<div class="card">
							<div class="card-body">
								<Pagination :total="pagTotal" v-model:page="pagPage" :page-length="pagPageLength" />
							</div>
							<div class="card-body">
								<pre>{{pagPage}}</pre>
							</div>
						</div>
					</div>

					<div class="card">
						<div class="card-body">
							<div class="row">
								<div class="col-6">
									<tab-panel>
										<Tab title="Содержимое сумочки" icon="ti ti-chart-histogram"> сорежимое </Tab>
										<Tab title="История" icon="ti ti-chart-histogram"> История </Tab>
									</tab-panel>
								</div>

								<div class="col-6">
									<p>табы</p>
								</div>
							</div>

							<div class="row">
								<div class="col-6">
									<tab-panel>
										<Tab title="Содержимое сумочки" icon="ti ti-chart-histogram"> сорежимое </Tab>
										<Tab title="История" icon="ti ti-chart-histogram"> История </Tab>
									</tab-panel>
								</div>

								<div class="col-6">
									<p>Вертикальные табы</p>
								</div>
							</div>

						</div>
					</div>

					<div class="card">
						<div class="card-body">
							<div class="row">
								<div class="col-3">
									<input type="date" class="form-control" v-model="dateFlatPicr">
								</div>
								<div class="col-3">
									<Flatpickr class="form-control" locale="ru" v-model="dateFlatPicr" />
								</div>

								<div class="col-6">
									<p>FlatPicr</p>
								</div>
							</div>
						</div>
					</div>


					<div class="card">
						<div class="card-body">
							<div class="row">
								<div class="col-3">
									<Offcanvas ref="offcanvasLeftDefault" :overlay="false" >
										Содержимое
									</Offcanvas>
									<Offcanvas ref="offcanvasLeftMd" size="md" >
										Содержимое 2
									</Offcanvas>
									<Offcanvas ref="offcanvasLeftXl" size="xl" >
										Содержимое 3
									</Offcanvas>
									<Offcanvas ref="offcanvasRightXl" size="md" position="end" >
										Содержимое 1 offcanvasRightXl
									</Offcanvas>
									<Offcanvas ref="offcanvasTopXl" size="md" position="top" >
										Падамем сверху
									</Offcanvas>
									<Offcanvas ref="offcanvasBottomXl" size="md" position="bottom" >
										Лезем снизу
									</Offcanvas>
								</div>
								<div class="col-3">

								</div>

								<div class="col-6">
									<p>Offcanvas</p>
									<button class="btn btn-outline-primary" @click="$refs.offcanvasLeftDefault.show()">left default</button>
									<button class="btn btn-outline-primary" @click="$refs.offcanvasLeftMd.show()">left Md</button>
									<button class="btn btn-outline-primary" @click="$refs.offcanvasLeftXl.show()">left xl</button>
									<button class="btn btn-outline-primary" @click="$refs.offcanvasRightXl.show()">right md</button>
									<button class="btn btn-outline-primary" @click="$refs.offcanvasTopXl.show()">top md</button>
									<button class="btn btn-outline-primary" @click="$refs.offcanvasBottomXl.show()">bottom md</button>
								</div>
							</div>
						</div>
					</div>





				</div>
			</div>
		</div>

		<!-- <footer class="footer footer-transparent d-print-none">
				<div class="container-xl">
					<div class="row text-center align-items-center flex-row-reverse">
						<div class="col-lg-auto ms-lg-auto">
							<ul class="list-inline list-inline-dots mb-0">

								<li class="list-inline-item"><a href="./docs/" class="link-secondary">Documentation</a></li>
								<li class="list-inline-item"><a href="./license.html" class="link-secondary">License</a></li>
								<li class="list-inline-item"><a href="https://github.com/tabler/tabler" target="_blank"
										class="link-secondary" rel="noopener">Source code</a></li>
								<li class="list-inline-item">
									<a href="https://github.com/sponsors/codecalm" target="_blank" class="link-secondary"
										rel="noopener">
										<svg xmlns="http://www.w3.org/2000/svg" class="icon text-pink icon-filled icon-inline"
											width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor"
											fill="none" stroke-linecap="round" stroke-linejoin="round">
											<path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
											<path d="M19.5 12.572l-7.5 7.428l-7.5 -7.428a5 5 0 1 1 7.5 -6.566a5 5 0 1 1 7.5 6.572">
											</path>
										</svg>
										Sponsor
									</a>
								</li>
							</ul>
						</div>
						<div class="col-12 col-lg-auto mt-3 mt-lg-0">
							<ul class="list-inline list-inline-dots mb-0">
								<li class="list-inline-item">
									Copyright © 2023
									<a href="." class="link-secondary">Tabler</a>.
									All rights reserved.
								</li>
								<li class="list-inline-item">
									<a href="./changelog.html" class="link-secondary" rel="noopener">
										v1.0.0-beta17
									</a>
								</li>
							</ul>
						</div>
					</div>
				</div>
			</footer> -->

	</div>

</template>

<style scoped>

</style>
