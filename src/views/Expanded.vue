<template>
  <div class="mx-0 md:mx-10 my-5 text-center">
    <router-link to="/" class="bg-blue-900 hover:bg-blue-600 text-white font-bold px-4 rounded-full py-2">&#8592; Back</router-link>
    <div class="flex flex-col m-5 py-2 justify-start text-center text-2x text-200 bg-white shadow-2xl border border-gray-900 rounded-md w-max-fit">
    <h1 class="mt-5 text-2xl">{{$route.params.title}}</h1>
    <p class="text-xs">{{$route.params.date}}</p>
    <iframe v-if="$route.params.media_type == 'video'"
            class="w-60 md:w-auto mx-auto"
            :src="$route.params.url"
            width="640"
            height="480"
            webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>
    <a v-else :href="$route.params.hdurl" target="_blank" alt="Click to open in full resolution">
        <img class="w-60 md:w-auto mx-auto mt-5 max-w-4xl" :src="$route.params.hdurl" alt="Click to open in full resolution" />
    </a>
    <p v-if="$route.params.copyright" class="text-xs">Copyright: {{$route.params.copyright}}</p>
    <p v-else class="text-xs">Copyright: NASA</p>
    <p class="grow text-sm my-2 mx-2 md:mx-auto max-w-4xl">{{$route.params.explanation}}</p>
</div>
</div>
</template>

<script>
export default {
  props: ['post']
}
</script>