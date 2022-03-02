<template>
	<div class="flex mt-5 flex-row justify-center w-min mx-auto">
		<input type="date" placeholder="MM/DD/YYYY" class="text-right p-2 mr-2 border-blue-500 border-2 rounded-full" v-model="searchDate" />
		<button @click="search" class="bg-blue-900 hover:bg-blue-600 text-white font-bold px-4 rounded-full">Search</button>
	</div>
	<div class="flex grow mt-5 flex-row justify-center mx-auto ">
		<button @click="randomSearch" class="bg-green-900 hover:bg-green-600 text-white font-bold py-2 px-4 rounded-full w-min whitespace-nowrap">Surprise me!</button>
	</div>
	<div class="px-5 grow flex flex-wrap justify-center flex-row">
		<Card v-for="post in posts" :post="post" :key="post.date" />
	</div>
	<div class="footer self-end text-center pt-5 text-white text-xs">
		<p>Powered by 
			<a class="underline hover:text-blue-800 visited:text-purple-600" href="https://vuejs.org/" target="_blank">Vue.js</a> and 
			<a class="underline hover:text-blue-800 visited:text-purple-600" href="https://github.com/nasa/apod-api" target="_blank">NASA</a></p>
	</div>
</template>

<script>
import {reactive, toRefs, computed} from "vue";
import Card from '@/components/Card.vue';

export default {
	name: 'Home',
	components: {
		Card
	},
	setup() {
		const state = reactive({
			posts: [],
			searchDate: "",
			refreshDate: computed(() => refreshDate())
		});

		// gets the date from the input, or if the input is empty, returns current date
		function refreshDate() {
			console.log('refreshing date!');
			if (!state.searchDate) {
				// input empty, return today's date
				let today = new Date();
				return sanitizeDate(today);
			}

			let searchDate = state.searchDate;
			return sanitizeDate(searchDate);
		}

		// sanitizes a date string
		function sanitizeDate(date) {
			var dd = String(date.getDate()).padStart(2, '0'),
			mm = String(date.getMonth() + 1).padStart(2, '0'),
			yyyy = date.getFullYear(),
			uglyDate = yyyy + "-" + mm + "-" + dd,
			niceDate = mm + '/' + dd + '/' + yyyy;

			state.searchDate = uglyDate;
			return niceDate;
		}

		// performs the search against the APOD api
		function search() {
			fetch("https://api.nasa.gov/planetary/apod?api_key=up0wVqmYq3dlP8SAGdRVWljjiELKm44P38DRGuNC&date=" + state.searchDate)
			.then((res) => res.json())
			.then((data) => {
				if (data.code) {
					// invalid date / no data, so build a dummy post
					data = {
						date: "undefined",// passed to niceDate in Card.vue
						explanation: "",
						media_type: "image",
						service_version: "v1",
						title: "Sorry, no picture for that date :(",
						url: "/einstein.jpeg",
						hdurl: "/einstein.jpeg",
						copyright: "None"
					}
				}
				console.log(data);
				state.posts = [data];
				console.dir(state);
			})
		}

		// returns a random 6 APOD entries
		function randomSearch() {
			fetch("https://api.nasa.gov/planetary/apod?api_key=up0wVqmYq3dlP8SAGdRVWljjiELKm44P38DRGuNC&count=6")
			.then((res) => res.json())
			.then((data) => {
				console.log(data);
				state.posts = data;
				console.dir(state);
			})
		}

		return {
			...toRefs(state),
			refreshDate,
			search,
			randomSearch
		}
	}
}
</script>
