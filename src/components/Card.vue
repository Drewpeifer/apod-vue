<template>
	<router-link
		class="flex flex-col m-5 p-5 justify-start text-center text-2x text-200 bg-white shadow-2xl border border-gray-900 rounded-md w-max-fit"
		:to="{name: 'Expanded', params: {
			title: post.title,
			date: niceDate(post.date),
			copyright: post.copyright,
			url: post.url,
			hdurl: post.hdurl,
			explanation: post.explanation,
			media_type: post.media_type
		}}">
		<div>
			<h1>{{post.title}}</h1>
			<p class="text-xs mb-2">{{ niceDate(post.date) }}</p>
			<iframe v-if="post.media_type == 'video'"
					class="mx-auto"
					:src="post.url"
					width="320"
					height="240"
					webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>
			<img v-else class="w-60 md:w-auto mx-auto max-w-xl" :src="post.url" />
			<p v-if="post.copyright" class="text-xs">Copyright: {{post.copyright}}</p>
			<p v-else class="text-xs">Copyright: NASA</p>
			<p class="text-xs max-w-xl mx-auto my-2 text-justify">{{post.explanation}}</p>
		</div>
	</router-link>
</template>

<script>
export default {
	data: (post) => ({
		date: 'post.date'
	}),
	props: ['post'],
	methods: {
		niceDate(date) {
			let arr = date.split('-'),
				y = arr[0],
				m = arr[1],
				d = arr[2],
				niceDate = m + '/' + d + '/' + y;

				if (date === 'undefined') {
					// when user asks for a date outside the API's range, we feed
					// a static string "undefined" to the obj, which should not be parsed,
					// and is replaced by an empty string for the UI
					niceDate = "(Search must fall between Jun 16, 1995 and Mar 02, 2022)";
				}

			return niceDate;
		}
	}
}
</script>