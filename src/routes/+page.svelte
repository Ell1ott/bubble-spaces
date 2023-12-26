<script lang="ts">
	import Hero from './hero.svelte';
	// export const prerender = true;

	let ref: HTMLElement;
</script>

<Hero></Hero>
<div class="main-content"></div>

{#each Array(100) as _, i}
	<div class="my-div">{i + 1}</div>
{/each}

<style lang="scss">
	@import url('https://fonts.googleapis.com/css2?family=League+Spartan:wght@900&display=swap');

	.main-content {
		// background-color: rgba(0, 0, 0, 0.226);
		background-color: #020b0ffe;
		z-index: 100;
		width: 100%;
		position: relative;
		backdrop-filter: blur(10px);

		height: 100vh;
	}

	:global(.fade-in) {
		animation: cubic-bezier(0.64, -0.03, 0.2, 0.97) fade-in 2s;
		animation-fill-mode: backwards;
	}

	@keyframes scale-in {
		from {
			scale: 0;
		}
		to {
			scale: 1;
		}
	}

	@keyframes fade-in {
		from {
			translate: 0 -20px;
			opacity: 0;
		}
	}
</style>
