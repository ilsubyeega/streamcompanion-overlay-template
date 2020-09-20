<script>
	import { onMount } from 'svelte';
	let ws;
	let data;
	import ReconnectingWebSocket from './reconnecting-websocket.js';
	onMount(() => {
		let initdata = [
			"mapStrains",
			"mapArtistTitle",
			"creator",
			"diffName",
			"mStars",
			"cs",
			"ar",
			"od",
			"hp",
			"time",
			"totaltime",
			"status",
			"c100",
			"c50",
			"miss"
		]
		ws = new ReconnectingWebSocket("ws://localhost:28390/tokens", null, {debug: true, reconnectInterval: 3000});
		ws.onopen = function (event) {
			console.log("Connected to websocket.")
			ws.send(JSON.stringify(initdata)); // send init data.
		}
		ws.onmessage = function (event) {
			data = {...data, ...JSON.parse(event.data)}; // merge data.
		}
	});
</script>



<main>
	{#if data != null} <!-- if data is null, its not connected or just bug(error) -->
		<p>Data: {JSON.stringify(data)}</p>
	{/if}
	
</main>
