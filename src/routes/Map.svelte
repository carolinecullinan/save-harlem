<script>
	import { onMount } from 'svelte';
	import mapboxgl from 'mapbox-gl';

	export let show;
	export let currentState;
    export let showMarker;
	export let showStreet;
	export let showEastHarlem;
	export let showDollarSigns;


	$: console.log(currentState);

	const MAPBOX_TOKEN =
		'pk.eyJ1IjoiY2Fyb2xpbmVjdWxsaW5hbiIsImEiOiJja2kzY2ZtbGYwZTBzMzBxc3duejUya2ppIn0.4qH1PeRAd_qdXGX-8MdhPw';
	mapboxgl.accessToken = MAPBOX_TOKEN;

    let marker; // marker for 125th street station
	let streetHighlight_main; // highlight for 125th St / MLK Jr Blvd
	let streetHighlight_diagonal; // highlight for diagonal portion of 125th St
	let eastHarlemHighlight; // highlight for East Harlem
	//let dollarSignsLayer; // dollar signs for rent prices
	let map;

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

	// create an East Harlem highlight object (similar to marker and streetHighlight)
	class EastHarlemHighlight {
		constructor(options = {}) {
			this.coordinates = options.coordinates;
			this.color = options.color || '#FF9B00';
			this.opacity = options.opacity || 0.2;
			this.sourceId = `area-${Math.random().toString(36).substring(7)}`;
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
							type: 'Polygon',
							coordinates: [this.coordinates]
						}
					}
				});
			}
		
		// add layer if it doesn't exist
		if (!map.getLayer(this.layerId)) {
			map.addLayer({
				id: this.layerId,
				type: 'fill',
				source: this.sourceId,
				paint: {
					'fill-color': this.color,
					'fill-opacity': this.opacity,
					'fill-outline-color': this.color,
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
	/// ^^^ testing EastHighlight class below ^^^
	/// ^^^ testing EastHighlight class above ^^^
	
	function addDollarSigns(map) {
        if (!map.getSource('dollar-signs')) {
            map.addSource('dollar-signs', {
                type: 'geojson',
                data: {
                    type: 'FeatureCollection',
                    features: [
                        { type: 'Feature', geometry: { type: 'Point', coordinates: [-73.9373, 40.7990] }, properties: {} },
                        { type: 'Feature', geometry: { type: 'Point', coordinates: [-73.9420, 40.7995] }, properties: {} },
                        { type: 'Feature', geometry: { type: 'Point', coordinates: [-73.9350, 40.8020] }, properties: {} },
                        { type: 'Feature', geometry: { type: 'Point', coordinates: [-73.9400, 40.8025] }, properties: {} }
                    ]
                }
            });

            map.addLayer({
                id: 'dollar-signs',
                type: 'symbol',
                source: 'dollar-signs',
                layout: {
                    'text-field': '$',
                    'text-size': 24,
                    'text-allow-overlap': true
                },
                paint: {
                    'text-color': '#FFD700'
                }
            });
        }
    }

    function removeDollarSigns(map) {
        if (map.getLayer('dollar-signs')) {
            map.removeLayer('dollar-signs');
            map.removeSource('dollar-signs');
        }
    }



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
            color: '#FF9B00',
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

		// create highlight of East Harlem
		eastHarlemHighlight = new EastHarlemHighlight({
			coordinates: [
				[-73.9326, 40.7857],  // SE - Near 96th St & FDR Drive
        		[-73.9323, 40.8016],  // E - Near 116th St & FDR Drive
        		[-73.9321, 40.8127],  // NE - Near Harlem River Drive
        		[-73.934282, 40.811572],  // N - Near 142nd St, -73.9387, 40.8183, -73.934282, 40.811572
        		[-73.938930, 40.813136],  // NW - Near 5th Ave & 142nd St, -73.9470, 40.8179
        		[-73.95574, 40.78796],  // SW - Near 5th Ave & 96th St (fixed to 5th Ave), -73.9589, 40.7947
        		[-73.9326, 40.7857]   // Back to start (SE)
		// 		 [-73.9326, 40.7857],  // SE - Near 96th St & FDR Drive, 
		// 		 [-73.9323, 40.8016],  // E - Near 116th St & FDR Drive
		// 		 [-73.9321, 40.8127],  // NE - Near Harlem River Drive
		// 		 [-73.9387, 40.8183],  // N - Near 142nd St
		// 		 [-73.9470, 40.8179],  // NW - Near 5th Ave & 142nd St
		// 		 [-73.9589, 40.7947],  // SW - Near 5th Ave & 96th St
		// 		 [-73.9326, 40.7857]   // Back to start (SE)
		 ],
			color: '#FF9B00',
			opacity: 0.2
		});	
	});


	// add or remove marker, streetHighlight, and EastHarlemHighlight based on the step
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
		// EastHarlemHighlight visibility handling
		if (showEastHarlem) {
			eastHarlemHighlight.addTo(map);
		} else {
			eastHarlemHighlight.remove();
		}
		// dollarSigns visibility handling
		if (showDollarSigns) {
			addDollarSigns(map);
		} else {
			removeDollarSigns(map);
        }

	}

	// logging to debug
	// $: console.log('map: ', map);
    $: console.log('marker: ', marker);
	// $: console.log('streetHighlight: ', streetHighlight);
	//$: console.log('eastHarlemHighlight: ', eastHarlemHighlight);
</script>

<div id="map"></div>

<style>
	#map {
		height: 100vh;
		width: 100vw;
	}

</style>
