<script>
	import { citys, height, width } from '$lib/93.json'
	import { getPathFromPoints } from '$lib/utils/getPathFromPoints.js'
	import { intersectionObserver } from '$lib/actions/intersectionObserver.js'
	import { sineOut } from 'svelte/easing'
	import { draw } from 'svelte/transition'

	/**code of the selected city
	 * @type {string|undefined}
	 */
	export let selected

	let visible = false
	let drawn = false

	function onScreenEnter() {
		if (visible) return
		visible = true
		setTimeout(() => (drawn = true), 2000)
	}
</script>

<svg
	role="figure"
	viewBox={`0 0 500 500`}
	class="ml-auto h-full max-h-full max-w-full"
	use:intersectionObserver={{ once: false, threshold: 0.5 }}
	on:screenEnter={onScreenEnter}
>
	{#each citys as { code, points }}
		{#if visible}
			<path
				in:draw={{ duration: 2000, easing: sineOut, delay: 0 }}
				id={code}
				class=" fill-gray-900 stroke-blue/30"
				d={getPathFromPoints(points, width, height)}
				stroke-linejoin="round"
			/>
		{/if}
		<path
			class="fill-blue/80 stroke-blue/60 stroke-[2] transition-opacity duration-500"
			class:opacity-0={selected !== code || !drawn}
			d={getPathFromPoints(points, width, height)}
			stroke-linejoin="round"
		/>
	{/each}
</svg>
