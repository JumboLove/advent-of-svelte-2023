<script lang="ts">
	import type { FormEventHandler } from 'svelte/elements';
	let inputLoanAmount = 0;
	let inputInterest = 0;
	let inputTerm = 0;

	let monthlyPayment = 0;
	let formattedPayment = '';
	$: formattedPayment = new Intl.NumberFormat('en-US', {
		style: 'currency',
		currency: 'USD'
	}).format(monthlyPayment);

	// M = P(i(1+i)n)/((1+i)n - 1)

	function handleFormChange() {
		if (inputLoanAmount && inputInterest && inputTerm) {
			monthlyPayment = calulateMortgage(inputLoanAmount, inputInterest, inputTerm);
		}
	}

	function calulateMortgage(loanAmount: number, interest: number, term: number) {
		let monthlyPayment = 0;
		let monthlyInterest = interest / 12;
		let totalPayments = term * 12;

		monthlyPayment =
			(loanAmount * (monthlyInterest * (1 + monthlyInterest) * totalPayments)) /
			((1 + monthlyInterest) * totalPayments - 1);

		return monthlyPayment;
	}
</script>

<main class="max-w-4xl mx-auto my-4 p-4">
	<h1 class="text-3xl pb-6">Mortgage Calculator</h1>

	<form action="" on:change={handleFormChange}>
		<div class="form-row">
			<label for="loanamount">Loan Amount</label>
			<input
				type="number"
				id="loanamount"
				name="loanamount"
				bind:value={inputLoanAmount}
				placeholder="0"
				min="0"
			/>
		</div>

		<div class="form-row">
			<label for="interest">Interest Rate</label>
			<input
				type="number"
				id="interest"
				name="interest"
				bind:value={inputInterest}
				placeholder="0"
				min="0"
			/>
		</div>

		<div class="form-row">
			<label for="loanterm">Loan Term</label>
			<input
				type="number"
				id="loanterm"
				name="loanterm"
				bind:value={inputTerm}
				placeholder="0"
				min="0"
			/>
		</div>
	</form>

	{#if monthlyPayment > 0}
		<div>
			<h2 class="text-3xl font-semi-bold">Monthly Payment</h2>
			<p>{formattedPayment}</p>
		</div>
	{/if}
</main>

<style lang="postcss">
	label {
		@apply font-semibold mr-2;
	}

	input {
		@apply p-2 outline outline-gray-200/50 rounded-md;
	}

	form {
		@apply max-w-[400px] space-y-6 my-4;
	}

	.form-row {
		@apply flex justify-end gap-4 items-center;
	}
</style>
