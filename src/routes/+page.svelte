<script lang="ts">
	// export const prerender = true;
	import { onMount } from 'svelte';

	let circles: { style: string }[] = [];

	let colors = ['#00828d', '#00548d', '#8d008a', '#008d6b'];

	console.log(circles);

	circles = circles;
	onMount(() => {
		for (let i = 0; i < 200; i++) {
			let colorIndex = Math.floor(Math.random() * 4);

			const z = Math.random() * 500;
			circles.push({
				style: `
				left: ${Math.random() * 100}%;
				top: ${Math.random() * 100}%;
				transform: translateZ(${z}px);
				opacity: ${z / 500};

				background-color: var(--circle-color-${colorIndex});
				
			`
			});
			circles = circles;
		}
	});
</script>

<div class="hero"></div>
<div class="circles">
	<div class="helper">
		<div class="center-text">
			<h1 class="bubble fade-in">Bubble</h1>
			<h1 class="spaces fade-in">Spaces</h1>
			<p class="fade-in">
				Discover Bubble Spaces – a dynamic realm of learning and coding innovation. Much more than a
				platform, it's a creative sanctuary where curiosity meets coding.
			</p>
		</div>
		{#each circles as { style }}
			<div class="circle" {style}></div>
		{/each}
	</div>
</div>

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
	.center-text {
		position: absolute;
		top: 50%;
		left: 50%;

		line-height: 25px;

		transform: translateZ(400px) translate(-50%, -50%);

		display: flex;
		flex-direction: column;
		align-items: center;

		text-align: center;
		h1 {
			font-family: 'League Spartan', sans-serif;
			margin: 30px;

			font-weight: 1000;
			font-size: 80px;
			letter-spacing: 5px;
		}

		p {
			color: rgba(255, 255, 255, 0.6);
			font-weight: 400;
			font-size: 15px;
			line-height: 18px;

			max-width: 400px;

			animation-delay: 300ms;
		}

		.bubble {
			transform: translate(25px, 40px);
			margin-top: -10px;
		}

		.spaces {
			animation-delay: 100ms;
			transform: translate(-25px, 0px);
		}
	}

	.circle {
		position: absolute;

		height: 30px;
		width: 30px;
		background-color: blue;
		border-radius: 100%;
		transform-style: preserve-3d;
	}

	.hero {
		height: 100vh;
	}

	.fade-in {
		animation: cubic-bezier(0.64, -0.03, 0.2, 0.97) fade-in 2s;
		animation-fill-mode: backwards;
	}

	.circles {
		--circle-color-1: #00828d;
		--circle-color-2: #00548d;
		--circle-color-3: #8d008a;
		--circle-color-4: #008d6b;
		position: fixed;
		left: 0;
		right: 0;
		top: 0;
		width: 100%;
		height: 100vh;

		/* transform: perspective(600px); */
		transform-style: preserve-3d;
		perspective: 1000px;
		perspective-origin: center;
	}

	.helper {
		height: 100%;
		width: 100%;
		animation: linear lol;
		/* animation-iteration-count: infinite; */
		/* animation-duration: 10s; */
		/* animation-timing-function: scroll(); */
		animation-timeline: scroll();
		animation-range-end: 10000px;
		transform-style: preserve-3d;
	}

	@keyframes lol {
		from {
			transform: translateY(0);
		}
		to {
			transform: translateY(-4000px);
		}
	}

	@keyframes fade-in {
		from {
			translate: 0 -20px;
			opacity: 0;
		}
	}
</style>
