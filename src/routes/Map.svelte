<script>
	import { onMount } from 'svelte';
	import mapboxgl from 'mapbox-gl';

	export let show;
	export let currentState;

	$: console.log(currentState);

	// Replace with your Mapbox token
	// const MAPBOX_TOKEN = import.meta.env.PUBLIC_MAPBOX_TOKEN;
	const MAPBOX_TOKEN =
		'pk.eyJ1IjoiY2Fyb2xpbmVjdWxsaW5hbiIsImEiOiJja2kzY2ZtbGYwZTBzMzBxc3duejUya2ppIn0.4qH1PeRAd_qdXGX-8MdhPw';

	mapboxgl.accessToken = MAPBOX_TOKEN;

	let map;
	let marker; //marker for 125th street station

	onMount(() => {
		// Initialize Mapbox

		map = new mapboxgl.Map({
			container: 'map',
			style: 'mapbox://styles/mapbox/dark-v10',
			center: [-74.006, 40.7128],
			zoom: 10,
			interactive: false
		});

		// create marker but don't show it yet
		marker = new mapboxgl.Marker().setLngLat([-73.9374, 40.8044]).addTo(map);
	});

	// add or remove marker based on the step

	$: if (map !== undefined) {
		map.flyTo(
			currentState
			// duration: 2000,
			// essential: true,
		);

		if (map.getCenter().lng === -73.9373 && map.getCenter().lat === 40.8044) {
			marker.addTo(map);
		} else {
			marker.remove(map);
		}
	}

	$: console.log('map', map);
</script>

<div id="map"></div>

<style>
	#map {
		height: 100vh;
		width: 100vw;
		/* background-color: aqua; */
	}
</style>
