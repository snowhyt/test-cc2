<script lang="ts">
	// @ts-nocheck

	import { onMount } from 'svelte';
	import { writable, derived } from 'svelte/store';
	let apiUrl = 'https://api.exchangerate-api.com/v4/latest/USD';

	let baseCurrency = writable('USD');
	let amount = writable(1);
	let searchQuery = writable('');
	let rates = writable({});
	let currencyList = derived(rates, ($rates) => Object.keys($rates));
	let filteredCurrencies = derived([currencyList, searchQuery], ([$currencyList, $searchQuery]) =>
		$currencyList.filter((currency) => currency.toLowerCase().includes($searchQuery.toLowerCase()))
	);

	onMount(async () => {
		try {
			const response = await fetch(apiUrl);
			const data = await response.json();
			rates.set(data.rates);
		} catch (error) {
			console.error('Error fetching currency data:', error);
		}
	});

	const formatNumber = new Intl.NumberFormat('en-US', {
		maximumFractionDigits: 2
	});
</script>

<main class="flex min-h-screen flex-col items-center bg-gray-100 p-6">
	<h1 class="mb-4 text-center text-6xl font-bold z-10 text-white" style="text-shadow: 0px 0px 30px #000000;">XchangeMate</h1>
	<div class="w-full max-w-2xl rounded-lg bg-white p-6 shadow-lg z-10 drop-shadow-[0_35px_35px_rgba(0,0,0,.8)] mt-5">
		<h1 class="mb-4 text-center text-xl font-bold">Currency Exchange</h1>

		<div class="mb-4">
			<!-- svelte-ignore a11y_label_has_associated_control -->
			<label class="block text-sm text-gray-600">Amount</label>
			<input type="number" bind:value={$amount} class="w-full rounded-md border p-2" />
		</div>

		<div class="mb-4">
			<!-- svelte-ignore a11y_label_has_associated_control -->
			<label class="block text-sm font-semibold text-gray-600">Base Currency</label>
			<select bind:value={$baseCurrency} class="w-full rounded-md border p-2">
				{#each $currencyList as currency}
					<option value={currency}>{currency}</option>
				{/each}
			</select>
		</div>

		<div class="mb-4">
			<!-- svelte-ignore a11y_label_has_associated_control -->
			<label class="block text-sm font-semibold text-gray-600">Search</label>
			<input
				type="text"
				bind:value={$searchQuery}
				class="w-full rounded-md border p-2"
				placeholder="Search currency..."
			/>
		</div>

		<ul class="mt-4 max-h-96 overflow-y-auto rounded-md border p-2">
			{#each $filteredCurrencies as currency}
				<li class="flex justify-between border-b p-2 last:border-0">
					<span class="font-medium">{currency}</span>
					<span class="text-gray-700"
						>{formatNumber.format(
							($amount * ($rates[currency] / $rates[$baseCurrency])).toFixed(2)
						)}</span
					>
				</li>
			{/each}
		</ul>
	</div>
	<div class="overlay z-0"></div>
</main>

<style>
	@keyframes gradientBG {
		0% {
			background-position: 0% 50%;
		}
		50% {
			background-position: 100% 50%;
		}
		100% {
			background-position: 0% 50%;
		}
	}

	@keyframes fadeIn {
		from {
			opacity: 0;
			transform: translateY(-20px);
		}
		to {
			opacity: 1;
			transform: translateY(0);
		}
	}

	@keyframes backgroundChange {
		0%,
		100% {
			background-image: url('/splashscreen/splash_bg1.jpg');
		}
		25% {
			background-image: url('/splashscreen/splash_bg2.jpg');
		}
		50% {
			background-image: url('/splashscreen/splash_bg3.jpg');
		}
		75% {
			background-image: url('/splashscreen/splash_bg4.jpg');
		}
	}

	main {
		animation: backgroundChange 30s infinite ease-in-out;
		background-size: cover;
		background-position: center;
	}
	.overlay {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background: linear-gradient(217deg, rgba(47, 0, 255, 0.8), rgba(255, 0, 0, 0) 70.71%),
			linear-gradient(127deg, rgba(53, 147, 255, 0.8), rgba(0, 255, 0, 0) 70.71%),
			linear-gradient(336deg, rgba(25, 0, 255, 0.8), rgba(0, 0, 255, 0) 70.71%);
	}
</style>
