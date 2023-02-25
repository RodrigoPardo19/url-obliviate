<script setup lang="ts">
defineProps<{ msg: string }>();
</script>

<template>
	<div class="flex flex-col items-center">
		<div class="bg-black p-3 rounded-full self-start -rotate-45">
			<p class="text-white font-semibold text-lg">Coming Soon!</p>
		</div>
		<img alt="Vue logo" class="logo" src="@/assets/owl.png" width="200" height="200" />
		<div class="bg-primary py-4 px-10 rounded-xl text-center">
			<h1 class="font-bold text-7xl text-black">{{ msg }}</h1>
		</div>
		<p class="text-xl mt-2">
			A simple web to remember
			<span class="bg-black rounded-xl px-2 text-primary font-medium">URLS</span>
		</p>
	</div>
</template>
