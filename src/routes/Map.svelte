<script>
	import { onMount } from 'svelte';
	import mapboxgl from 'mapbox-gl';
	import { LeafyGreen } from 'lucide-svelte';

	export let show;
	export let currentState;
    export let showMarker;
	export let showStreet;
	export let show116Street;
	export let show3rdAvenue;
	export let showEastHarlem;
	export let showDollarSigns;


	$: console.log(currentState);

	const MAPBOX_TOKEN =
		'pk.eyJ1IjoiY2Fyb2xpbmVjdWxsaW5hbiIsImEiOiJja2kzY2ZtbGYwZTBzMzBxc3duejUya2ppIn0.4qH1PeRAd_qdXGX-8MdhPw';
	mapboxgl.accessToken = MAPBOX_TOKEN;

    let marker; // marker for 125th street station
	let streetHighlight_main; // highlight for 125th St / MLK Jr Blvd
	let streetHighlight_diagonal; // highlight for diagonal portion of 125th St
	let streetHighlight_116th; // highlight for 116th St
	let streetHighlight_3rdAvenue; // highlight for 3rd Avenue
	let eastHarlemHighlight; // highlight for East Harlem
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
					{ type: 'Feature', geometry: { type: 'Point', coordinates: [-73.939030, 40.808622] }, properties: {} },
					{ type: 'Feature', geometry: { type: 'Point', coordinates: [-73.944180, 40.790754] }, properties: {} },
					{ type: 'Feature', geometry: { type: 'Point', coordinates: [-73.937828, 40.791794] }, properties: {} },
					{ type: 'Feature', geometry: { type: 'Point', coordinates: [-73.952935, 40.789194] }, properties: {} },
					{ type: 'Feature', geometry: { type: 'Point', coordinates: [-73.940947, 40.796750] }, properties: {} },
					{ type: 'Feature', geometry: { type: 'Point', coordinates: [-73.941750, 40.793059] }, properties: {} },
					{ type: 'Feature', geometry: { type: 'Point', coordinates: [-73.937370, 40.795589] }, properties: {} },
					{ type: 'Feature', geometry: { type: 'Point', coordinates: [-73.934412, 40.805475] }, properties: {} },
					{ type: 'Feature', geometry: { type: 'Point', coordinates: [-73.946156, 40.795138] }, properties: {} },
					{ type: 'Feature', geometry: { type: 'Point', coordinates: [-73.945604, 40.801483] }, properties: {} },
				
					{ type: 'Feature', geometry: { type: 'Point', coordinates: [-73.942002, 40.806266] }, properties: {} },
					{ type: 'Feature', geometry: { type: 'Point', coordinates: [-73.935964, 40.806677] }, properties: {} },
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
                'text-color': '#FFD700',
                'text-translate': [0, 0]
            }
        });

        // Add animation
        let step = 0;
        const animate = () => {
            step += 0.1;
            const offset = Math.sin(step) * 10;
            
            if (map.getLayer('dollar-signs')) {
                map.setPaintProperty('dollar-signs', 'text-translate', [0, offset]);
                requestAnimationFrame(animate);
            }
        };
        
        animate();
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

		// create streetHighlight_116th but don't show it yet
		streetHighlight_116th = new StreetHighlight({
			coordinates: [
				[-73.946369, 40.800631], // western street point
				//[-73.931911, 40.794558] // eastern street point
				[-73.932852, 40.794917]
		],
			color: '#FF9B00',
			width: 8
		});

		// create streetHighlight_3rdAvenue but don't show it yet
		streetHighlight_3rdAvenue = new StreetHighlight({
			coordinates: [
				//[-73.944595, 40.791800], // southern point (keep as is)
				//[-73.934926, 40.804998]  // northern point (shifted east to create angle)	
				[-73.948300, 40.7870000], // southern point (keep as is) -73.944595, 40.791800
				[-73.934110, 40.806600] 
		  ],  // northern point (shifted east to create angle)	],
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
		// streetHighlight_116th visibility handling
		if (show116Street) {
			streetHighlight_116th.addTo(map);
		} else {
			streetHighlight_116th.remove();
		}
		// streetHighlight_3rdAvenue visibility handling
		if (show3rdAvenue) {
			streetHighlight_3rdAvenue.addTo(map);
		} else {
			streetHighlight_3rdAvenue.remove();
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
