<script lang="ts">
	import { Cidr, IpAddress, IpRange } from 'cidr-calc';

	let cidr = $state('');
	let startingIp: string = $state('');
	let endingIp: string = $state('');

	function calculateIpRange() {
		try {
			let parsedCIDR = new Cidr(IpAddress.of(cidr.split('/')[0]), parseInt(cidr.split('/')[1]));
			startingIp = parsedCIDR.toIpRange().startIpAddr.toString();
			endingIp = parsedCIDR.toIpRange().endIpAddr.toString();
		} catch (e) {
			startingIp = 'Invalid CIDR';
			endingIp = 'Invalid CIDR';
			console.error(e);
		}
	}

	function updateCidrFromIpRange() {
		try {
			let ipRange = new IpRange(IpAddress.of(startingIp), IpAddress.of(endingIp));

			cidr = ipRange.toCidrs().toString();
		} catch (e) {
			cidr = 'Invalid IP range';
			console.error(e);
		}
	}

	function copyToClipboard(text: string) {
		if (text && text !== 'Invalid CIDR') {
			navigator.clipboard.writeText(text).then(
				() => alert('Copied to clipboard!'),
				() => alert('Failed to copy.')
			);
		} else {
			alert('Nothing to copy.');
		}
	}

	function resetForm() {
		cidr = '';
		startingIp = '';
		endingIp = '';
	}

	function selectAll(event: Event) {
		(event.target as HTMLInputElement).select();
	}
</script>

<div class="flex min-h-screen flex-col items-center justify-center bg-gray-200">
	<div class="w-full max-w-md rounded-lg bg-white p-8 shadow-2xl">
		<h2 class="mb-6 text-center text-2xl font-bold text-gray-800">CIDR Calculator</h2>
		<form class="space-y-4">
			<div>
				<label for="cidr" class="block font-bold text-gray-600">CIDR</label>
				<div class="flex items-center space-x-2">
					<input
						type="text"
						id="cidr"
						bind:value={cidr}
						placeholder="e.g., 192.168.0.0/24"
						autocomplete="off"
						class="mt-1 w-full rounded-md border border-gray-300 px-4 py-2 focus:outline-none"
						onclick={selectAll}
					/>
					<button
						type="button"
						onclick={() => copyToClipboard(cidr)}
						class="rounded-md border border-gray-500 px-4 py-2 text-gray-500 hover:bg-gray-100 focus:outline-none focus:ring focus:ring-gray-300"
					>
						Copy
					</button>
				</div>
			</div>
			<div class="mt-2 flex space-x-2">
				<button
					type="button"
					onclick={() => calculateIpRange()}
					class="flex-1 rounded-md bg-blue-500 px-4 py-2 text-white hover:bg-blue-600 focus:outline-none focus:ring focus:ring-blue-300"
				>
					&#x2193; Calculate IP Range
				</button>
				<button
					type="button"
					onclick={updateCidrFromIpRange}
					class="flex-1 rounded-md bg-green-500 px-4 py-2 text-white hover:bg-green-600 focus:outline-none focus:ring focus:ring-green-300"
				>
					&#x2191; Update CIDR
				</button>
			</div>

			<div>
				<label for="startingIp" class="block font-bold text-gray-600">Starting IP</label>
				<div class="flex items-center space-x-2">
					<input
						type="text"
						id="startingIp"
						autocomplete="off"
						bind:value={startingIp}
						class="mt-1 w-full rounded-md border border-gray-300 px-4 py-2 focus:outline-none"
						onclick={selectAll}
					/>
					<button
						type="button"
						onclick={() => copyToClipboard(startingIp)}
						class="rounded-md border border-gray-500 px-4 py-2 text-gray-500 hover:bg-gray-100 focus:outline-none focus:ring focus:ring-gray-300"
					>
						Copy
					</button>
				</div>
			</div>

			<div>
				<label for="endingIp" class="block font-bold text-gray-600">Ending IP</label>
				<div class="flex items-center space-x-2">
					<input
						type="text"
						id="endingIp"
						autocomplete="off"
						bind:value={endingIp}
						class="mt-1 w-full rounded-md border border-gray-300 px-4 py-2 focus:outline-none"
						onclick={selectAll}
					/>
					<button
						type="button"
						onclick={() => copyToClipboard(endingIp)}
						class="rounded-md border border-gray-500 px-4 py-2 text-gray-500 hover:bg-gray-100 focus:outline-none focus:ring focus:ring-gray-300"
					>
						Copy
					</button>
				</div>
			</div>

			<button
				type="button"
				onclick={resetForm}
				class="mt-4 w-full rounded-md bg-red-500 px-4 py-2 text-white hover:bg-red-600 focus:outline-none focus:ring focus:ring-red-300"
			>
				Clear
			</button>
		</form>
	</div>
	<div class="mt-4 block w-full text-center text-gray-500">
		source: <a href="https://github.com/zeckon/cidr-calculator" class="underline"
			>https://github.com/zeckon/cidr-calculator</a
		>
	</div>
</div>
