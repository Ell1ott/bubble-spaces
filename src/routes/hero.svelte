<script lang="ts">
	import { onMount } from 'svelte';

	let circles: { style: string }[] = [];

	let colors = ['#00828d', '#00548d', '#8d008a', '#008d6b'];
	function sleep(ms: number) {
		return new Promise((resolve) => setTimeout(resolve, ms));
	}

	function calculateCircelCount() {
		const circelAmount = (window.innerWidth * window.innerHeight) / 1000 + 10;
		return Math.min(400, Math.floor(circelAmount));
	}

	function addCircel() {
		let colorIndex = Math.floor(Math.random() * 4);

		const z = Math.random() * 500;

		circles.push({
			style: `left: ${Math.random() * 100}%;
				top: ${Math.random() * 100}%;
				transform: translateZ(${z}px);
				opacity: ${z / 500};
				background-color: var(--circle-color-${colorIndex});`
		});
	}

	let perspectiveStyle = 'perspective-origin: center;';

	let msx = 0;
	let msy = 0;

	function updatePerspective(mouseSens: number) {
		const x = msx * mouseSens + window.innerWidth * (0.5 - mouseSens / 2) + window.scrollX;
		const y = msy * mouseSens + window.innerHeight * (0.5 - mouseSens / 2) + window.scrollY;
		perspectiveStyle = `perspective-origin: ${x}px ${y}px`;
	}
	onMount(async () => {
		msx = window.innerWidth / 2;
		msy = window.innerHeight / 2;
		window.addEventListener('mousemove', (ev) => {
			msx = ev.clientX;
			msy = ev.clientY;
			updatePerspective(-0.3);
		});

		window.addEventListener('scroll', (ev) => {
			updatePerspective(-0.3);
		});

		window.addEventListener('resize', async () => {
			const optimalCircleCount = calculateCircelCount();
			const currentCircleCount = circles.length;
			if (optimalCircleCount == currentCircleCount) {
				return;
			}

			if (optimalCircleCount < currentCircleCount) {
				circles = circles.slice(0, optimalCircleCount);
			}

			if (optimalCircleCount > currentCircleCount) {
				console.log('rezise');
				for (let i = 0; i < optimalCircleCount - currentCircleCount; i++) {
					if (optimalCircleCount != calculateCircelCount()) {
						break;
					}
					addCircel();

					circles = circles;
					await sleep(1);
				}
			}
		});
		console.log(window.innerHeight);

		console.log(calculateCircelCount());

		for (let i = 0; i < calculateCircelCount(); i++) {
			// filter: blur(${Math.abs(400 - z) / 100}px);
			addCircel();

			await sleep(1);
			circles = circles;
		}
	});
</script>

<div class="hero">
	<div class="circles" style={perspectiveStyle}>
		<div class="helper">
			<div class="center-text">
				<h1 class="bubble fade-in">Bubble</h1>
				<h1 class="spaces fade-in">Spaces</h1>
				<p class="fade-in">
					Discover Bubble Spaces – a dynamic realm of learning and coding innovation. Much more than
					a platform, it's a creative sanctuary where curiosity meets coding.
				</p>
			</div>
			{#each circles as { style }}
				<div class="circle" {style}></div>
			{/each}
			<div class="center-text copy">
				<h1 class="bubble fade-in">Bubble</h1>
				<h1 class="spaces fade-in">Spaces</h1>
				<p class="fade-in">
					Discover Bubble Spaces – a dynamic realm of learning and coding innovation. Much more than
					a platform, it's a creative sanctuary where curiosity meets coding.
				</p>
			</div>
		</div>
	</div>
</div>

<style lang="scss">
	.center-text {
		max-width: 50%;
		position: absolute;
		top: 50%;
		left: 50%;

		line-height: 1.1em;

		transform: translateZ(400px) translate(-50%, -50%);

		display: flex;
		flex-direction: column;
		align-items: center;

		text-align: center;
		font-size: min(50px, 6vw);
		h1 {
			font-family: 'League Spartan', sans-serif;
			margin: 0;
			font-weight: 1000;
			// letter-spacing: 5px;
			letter-spacing: 0.05em;
		}

		p {
			color: rgba(255, 255, 255, 0.6);
			font-weight: 400;
			font-size: min(15px, 3vw);
			line-height: 1em;

			max-width: 400px;

			animation-delay: 300ms;

			margin-top: 30px;

			@media screen and (max-height: 600px) {
				display: none;
			}
		}

		.bubble {
			transform: translate(0.28em, 0);
			margin-top: -10px;
		}

		.spaces {
			animation-delay: 100ms;
			transform: translate(-0.28em, 0px);
		}
	}

	.center-text.copy {
		transform: translateZ(350px) translate(calc(-50% + 10px), calc(-50% + 10px));
		filter: brightness(0) saturate(0) blur(5px);
		opacity: 0.2;
	}
	.circle {
		position: absolute;

		height: 30px;
		width: 30px;
		background-color: blue;
		border-radius: 100%;
		transform-style: preserve-3d;

		animation: cubic-bezier(0, 1.18, 0.39, 0.97) scale-in 1s;
	}

	.circles {
		--circle-color-1: #00828d;
		--circle-color-2: #00548d;
		--circle-color-3: #8d008a;
		--circle-color-4: #008d6b;
		// position: fixed;
		left: 0;
		right: 0;
		top: 0;
		width: 100%;
		height: 100vh;
		overflow: hidden;
		/* transform: perspective(600px); */
		transform-style: preserve-3d;
		perspective: 1000px;
	}

	.hero {
		height: 100vh;
	}

	.helper {
		height: 100%;
		width: 100%;
		// overflow: hidden;
		// animation: linear parralax;
		/* animation-iteration-count: infinite; */
		/* animation-duration: 10s; */
		/* animation-timing-function: scroll(); */
		animation-timeline: scroll();
		animation-range-end: 10000px;
		transform-style: preserve-3d;
	}

	@keyframes scale-in {
		from {
			scale: 0;
		}
		to {
			scale: 1;
		}
	}

	@keyframes parralax {
		from {
			transform: translateY(0);
		}
		to {
			transform: translateY(-4000px);
		}
	}
</style>
