<template>
	<router-link
		class="flex flex-col m-5 p-5 justify-start text-center text-2x text-200 bg-white shadow-2xl border border-gray-900 rounded-md w-max-fit"
		:to="{name: 'Expanded', params: {
			title: post.title,
			date: post.date,
			copyright: post.copyright,
			hdurl: post.hdurl,
			explanation: post.explanation
		}}">
		<div>
			<h1>{{post.title}}</h1>
			<p class="text-xs">{{post.date}}</p>
			<img class="mx-auto max-w-xl" :src="post.url" />
			<p v-if="post.copyright" class="text-xs">Copyright: {{post.copyright}}</p>
			<p v-else class="text-xs">Copyright: NASA</p>
			<p class="text-xs max-w-xl mx-auto my-2 text-justify">{{post.explanation}}</p>
		</div>
	</router-link>
</template>

<script>
export default {
	props: ['post']
}
</script>