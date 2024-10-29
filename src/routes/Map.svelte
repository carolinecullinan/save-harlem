<script>
	import { onMount } from 'svelte';
	import mapboxgl from 'mapbox-gl';

	export let show;
	export let currentState;
    export let showMarker;
	export let showStreet;

	$: console.log(currentState);

	const MAPBOX_TOKEN =
		'pk.eyJ1IjoiY2Fyb2xpbmVjdWxsaW5hbiIsImEiOiJja2kzY2ZtbGYwZTBzMzBxc3duejUya2ppIn0.4qH1PeRAd_qdXGX-8MdhPw';
	mapboxgl.accessToken = MAPBOX_TOKEN;

    let marker; // marker for 125th street station
	let streetHighlight_main; // highlight for 125th St / MLK Jr Blvd
	let streetHighlight_diagonal; // highlight for diagonal portion of 125th St
	let map;




	/// ^^^ testing marker class below ^^^

	// create a street highlight object (similar to marker)
	class StreetHighlight {
		constructor(options = {}) {
			this.coordinates = options.coordinates;
			this.color = options.color || '#FF9B00';
			this.width = options.width || 8;
			this.opacity = options.opacity || 0.5;
			this.blur = options.blur || 1;
			this.sourceId = `street-${Math.random().toString(36).substring(7)}`;
			this.layerId = `${this.sourceId}-layer`;
		}

		setCoordinates(coordinates) {
			this.coordinates = coordinates;
			return this;
		}

		addTo(map) {
			// add source if it doesn't exist
			if (!map.getSource(this.sourceId)) {
				map.addSource(this.sourceId, {
					type: 'geojson',
					data: {
						type: 'Feature',
						properties: {},
						geometry: {
							type: 'LineString',
							coordinates: this.coordinates
						}
					}
				});
			}
		
		// add layer if it doesn't exist
		if (!map.getLayer(this.layerId)) {
			map.addLayer({
				id: this.layerId,
				type: 'line',
				source: this.sourceId,
				paint: {
					'line-color': this.color,
					'line-width': this.width,
					'line-opacity': this.opacity,
					'line-blur': this.blur
				}
			});
		}

			return this;
		}

		remove() {
			if (map.getLayer(this.layerId)) {
				map.removeLayer(this.layerId);
			}
			if (map.getSource(this.sourceId)) {
				map.removeSource(this.sourceId);
			}
		}

	}
	/// ^^^ testing streetHighlight class above ^^^
	
	
	onMount(() => {
		// initialize Mapbox
		map = new mapboxgl.Map({
			container: 'map',
			style: 'mapbox://styles/mapbox/dark-v10',
			center: [-74.006, 40.7128],
			zoom: 10,
			interactive: false,   
		});

		// create marker but don't show it yet
		marker = new mapboxgl.Marker({
            color: 'teal',
        }).setLngLat([-73.9374, 40.8044]);

		// create streetHighlight_main but don't show it yet
		streetHighlight_main = new StreetHighlight({
			coordinates: [
				[-73.95481, 40.81170], // western street point
				[-73.93146, 40.80189] // eastern street point
		],
			color: '#FF9B00',
			width: 8
		});

		// create another streetHighlight_diagonal - diagonal portion of 125th St
		streetHighlight_diagonal = new StreetHighlight({
			coordinates: [
				[-73.96090, 40.81850], // western street point
				[-73.95481, 40.81170] // eastern street point
		],
			color: '#FF9B00',
			width: 8
		});

	});

	// add or remove marker and streetHighlight based on the step
	$: if (map !== undefined) {
		map.flyTo(
			currentState
			// duration: 2000,
			//essential: true,
		);
		// marker visibility handling
        if (showMarker) {
            marker.addTo(map);
        } else {
            marker.remove();
        }
		// streetHighlight visibility handling
		if (showStreet) {
			streetHighlight_main.addTo(map);
			streetHighlight_diagonal.addTo(map);
		} else {
			streetHighlight_main.remove();
			streetHighlight_diagonal.remove();
		}

	}

	// logging to debug
	// $: console.log('map: ', map);
    $: console.log('marker: ', marker);
	// $: console.log('streetHighlight: ', streetHighlight);

</script>

<div id="map"></div>

<style>
	#map {
		height: 100vh;
		width: 100vw;
	}

</style>
