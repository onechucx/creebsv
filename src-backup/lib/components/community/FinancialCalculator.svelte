<script lang="ts">
	import { community } from '$lib/stores';
	import Card from '$lib/components/common/Card.svelte';
	import Button from '$lib/components/common/Button.svelte';
	import {
		PieChart,
		Pie,
		Cell,
		LineChart,
		Line,
		XAxis,
		YAxis,
		CartesianGrid,
		Tooltip,
		ResponsiveContainer,
		Legend
	} from 'recharts';
	import { onMount } from 'svelte';

	let calculatorMode: 'loan' | 'savings' = 'loan';

	const { loanProducts, piggyProducts } = $community;

	// Loan State
	let loanType = loanProducts[0].id;
	let loanAmount = 500000;
	let loanTenure = 24;

	// Savings State
	let savingsProductId = piggyProducts[0]?.id || '';
	let initialDeposit = 50000;
	let monthlyContribution = 10000;
	let savingsTenure = 12;

	let selectedLoanProduct = loanProducts[0];
	let emi = 0;
	let totalPayable = 0;
	let totalInterest = 0;
	let loanChartData: { name: string; value: number }[] = [];
	let amortization: {
		month: number;
		principal: number;
		interest: number;
		balance: number;
	}[] = [];

	$: selectedLoanProduct = loanProducts.find((p) => p.id === loanType) || loanProducts[0];

	$: {
		const principal = loanAmount;
		const rate = selectedLoanProduct.interestRate / 100 / 12;
		const tenure = loanTenure;
		if (principal > 0 && rate > 0 && tenure > 0) {
			const calculatedEmi =
				(principal * rate * Math.pow(1 + rate, tenure)) / (Math.pow(1 + rate, tenure) - 1);
			const calculatedTotalPayable = calculatedEmi * tenure;
			const calculatedTotalInterest = calculatedTotalPayable - principal;

			emi = calculatedEmi;
			totalPayable = calculatedTotalPayable;
			totalInterest = calculatedTotalInterest;
			loanChartData = [
				{ name: 'Principal', value: principal },
				{ name: 'Interest', value: calculatedTotalInterest }
			];

			const schedule = [];
			let balance = principal;
			for (let i = 1; i <= tenure; i++) {
				const interestPayment = balance * rate;
				const principalPayment = calculatedEmi - interestPayment;
				balance -= principalPayment;
				schedule.push({
					month: i,
					principal: principalPayment,
					interest: interestPayment,
					balance: Math.max(0, balance)
				});
			}
			amortization = schedule;
		}
	}

	let selectedSavingsProduct = piggyProducts[0];
	let futureValue = 0;
	let totalContributions = 0;
	let totalInterestEarned = 0;
	let savingsPieData: { name: string; value: number }[] = [];
	let savingsGrowthData: { month: number; value: number }[] = [];

	$: selectedSavingsProduct = piggyProducts.find((p) => p.id === savingsProductId) || piggyProducts[0];

	$: {
		if (selectedSavingsProduct) {
			if (savingsTenure < selectedSavingsProduct.minTenure)
				savingsTenure = selectedSavingsProduct.minTenure;
			if (savingsTenure > selectedSavingsProduct.maxTenure)
				savingsTenure = selectedSavingsProduct.maxTenure;
		}
	}

	$: {
		if (selectedSavingsProduct) {
			const rate = selectedSavingsProduct.interestRate / 100 / 12;
			const n = savingsTenure;
			const P = initialDeposit;
			const PMT = monthlyContribution;

			const calculatedFutureValue =
				P * Math.pow(1 + rate, n) + PMT * ((Math.pow(1 + rate, n) - 1) / rate);
			const calculatedTotalContributions = P + PMT * n;
			const calculatedTotalInterest = calculatedFutureValue - calculatedTotalContributions;

			futureValue = calculatedFutureValue;
			totalContributions = calculatedTotalContributions;
			totalInterestEarned = calculatedTotalInterest;
			savingsPieData = [
				{ name: 'Contributions', value: calculatedTotalContributions },
				{ name: 'Interest', value: calculatedTotalInterest }
			];

			const growthData = [];
			let balance = P;
			growthData.push({ month: 0, value: P });
			for (let i = 1; i <= n; i++) {
				balance = balance * (1 + rate) + PMT;
				growthData.push({ month: i, value: balance });
			}
			savingsGrowthData = growthData;
		}
	}
</script>

<Card>
	<div class="p-4">
		<div class="flex justify-between items-center mb-4">
			<h3 class="text-xl font-bold text-brand-text-primary">Financial Calculator</h3>
			<div class="flex items-center bg-gray-200 dark:bg-gray-700 rounded-lg p-1">
				<Button
					on:click={() => (calculatorMode = 'loan')}
					class="!text-xs !py-1 !px-3 {calculatorMode === 'loan'
						? 'bg-brand-surface dark:bg-dark-surface shadow'
						: 'bg-transparent border-transparent'}">Loan</Button
				>
				<Button
					on:click={() => (calculatorMode = 'savings')}
					class="!text-xs !py-1 !px-3 {calculatorMode === 'savings'
						? 'bg-brand-surface dark:bg-dark-surface shadow'
						: 'bg-transparent border-transparent'}">Savings</Button
				>
			</div>
		</div>
		{#if calculatorMode === 'loan'}
			<div class="grid grid-cols-1 md:grid-cols-2 gap-8">
				<div class="space-y-6">
					<select
						bind:value={loanType}
						class="w-full p-3 border rounded-md bg-gray-100 dark:bg-gray-800 dark:border-dark-border text-brand-text-primary"
					>
						{#each loanProducts as p}
							<option value={p.id}>{p.name} ({p.interestRate}% APR)</option>
						{/each}
					</select>
					<div>
						<div class="flex justify-between font-semibold text-brand-text-primary">
							<label for="loanAmount">Loan Amount</label>
							<span>₦{loanAmount.toLocaleString()}</span>
						</div>
						<input
							type="range"
							id="loanAmount"
							min="50000"
							max={selectedLoanProduct.maxAmount}
							step="10000"
							bind:value={loanAmount}
							class="w-full"
						/>
					</div>
					<div>
						<div class="flex justify-between font-semibold text-brand-text-primary">
							<label for="loanTenure">Loan Tenure (Months)</label>
							<span>{loanTenure} months</span>
						</div>
						<input
							type="range"
							id="loanTenure"
							min="6"
							max={selectedLoanProduct.maxTenure}
							step="1"
							bind:value={loanTenure}
							class="w-full"
						/>
					</div>
					<div class="space-y-2 text-sm border-t dark:border-dark-border pt-4">
						<div class="flex justify-between">
							<span class="text-brand-text-secondary">Monthly Payment (EMI)</span>
							<span class="font-semibold text-brand-text-primary"
								>₦{emi.toLocaleString('en-US', {
									minimumFractionDigits: 2,
									maximumFractionDigits: 2
								})}</span
							>
						</div>
						<div class="flex justify-between">
							<span class="text-brand-text-secondary">Total Interest Payable</span>
							<span class="font-semibold text-brand-text-primary"
								>₦{totalInterest.toLocaleString('en-US', {
									minimumFractionDigits: 2,
									maximumFractionDigits: 2
								})}</span
							>
						</div>
						<div class="flex justify-between text-lg font-bold text-brand-text-primary">
							<span>Total Repayment</span>
							<span
								>₦{totalPayable.toLocaleString('en-US', {
									minimumFractionDigits: 2,
									maximumFractionDigits: 2
								})}</span
							>
						</div>
					</div>
					<Button class="w-full !py-3 text-base">Apply for Loan</Button>
				</div>
				<div>
					<ResponsiveContainer width="100%" height={200}>
						<PieChart>
							<Pie
								data={loanChartData}
								dataKey="value"
								nameKey="name"
								cx="50%"
								cy="50%"
								innerRadius={50}
								outerRadius={70}
								paddingAngle={5}
								labelLine={false}
								label={({ name, percent }) => `${(percent * 100).toFixed(0)}%`}
							>
								<Cell fill="var(--brand-primary)" />
								<Cell fill="#EF4444" />
							</Pie>
							<Tooltip />
							<Legend iconType="circle" />
						</PieChart>
					</ResponsiveContainer>
					<div class="h-48 overflow-y-auto mt-4 border rounded-md dark:border-dark-border">
						<table class="w-full text-xs text-center">
							<thead class="sticky top-0 bg-blue-100 dark:bg-blue-900/50">
								<tr class="text-blue-900 dark:text-blue-200 font-semibold">
									<th class="p-2">Month</th>
									<th class="p-2">Principal</th>
									<th class="p-2">Interest</th>
									<th class="p-2">Balance</th>
								</tr>
							</thead>
							<tbody
								class="text-brand-text-primary dark:text-dark-text-primary divide-y divide-brand-border dark:divide-dark-border"
							>
								{#each amortization as row (row.month)}
									<tr>
										<td class="p-2">{row.month}</td>
										<td class="p-2">{row.principal.toFixed(2)}</td>
										<td class="p-2">{row.interest.toFixed(2)}</td>
										<td class="p-2">{row.balance.toFixed(2)}</td>
									</tr>
								{/each}
							</tbody>
						</table>
					</div>
				</div>
			</div>
		{:else}
			<div class="grid grid-cols-1 md:grid-cols-2 gap-8">
				<div class="space-y-6">
					<select
						bind:value={savingsProductId}
						class="w-full p-3 border rounded-md bg-gray-100 dark:bg-gray-800 dark:border-dark-border text-brand-text-primary"
					>
						{#each piggyProducts as p}
							<option value={p.id}>{p.name} ({p.interestRate}% p.a.)</option>
						{/each}
					</select>
					<div>
						<div class="flex justify-between font-semibold text-brand-text-primary">
							<label for="initialDeposit">Initial Deposit</label>
							<span>₦{initialDeposit.toLocaleString()}</span>
						</div>
						<input
							type="range"
							id="initialDeposit"
							min="10000"
							max="1000000"
							step="10000"
							bind:value={initialDeposit}
							class="w-full"
						/>
					</div>
					<div>
						<div class="flex justify-between font-semibold text-brand-text-primary">
							<label for="monthlyContribution">Monthly Contribution</label>
							<span>₦{monthlyContribution.toLocaleString()}</span>
						</div>
						<input
							type="range"
							id="monthlyContribution"
							min="0"
							max="200000"
							step="5000"
							bind:value={monthlyContribution}
							class="w-full"
						/>
					</div>
					<div>
						<div class="flex justify-between font-semibold text-brand-text-primary">
							<label for="savingsTenure">Savings Tenure (Months)</label>
							<span>{savingsTenure} months</span>
						</div>
						<input
							type="range"
							id="savingsTenure"
							min={selectedSavingsProduct?.minTenure || 3}
							max={selectedSavingsProduct?.maxTenure || 48}
							step="1"
							bind:value={savingsTenure}
							class="w-full"
						/>
					</div>
					<div class="space-y-2 text-sm border-t dark:border-dark-border pt-4">
						<div class="flex justify-between">
							<span class="text-brand-text-secondary">Total Contributions</span>
							<span class="font-semibold text-brand-text-primary"
								>₦{totalContributions.toLocaleString('en-US', {
									minimumFractionDigits: 2,
									maximumFractionDigits: 2
								})}</span
							>
						</div>
						<div class="flex justify-between">
							<span class="text-brand-text-secondary">Total Interest Earned</span>
							<span class="font-semibold text-brand-text-primary"
								>₦{totalInterestEarned.toLocaleString('en-US', {
									minimumFractionDigits: 2,
									maximumFractionDigits: 2
								})}</span
							>
						</div>
						<div class="flex justify-between text-lg font-bold text-brand-text-primary">
							<span>Future Value</span>
							<span
								>₦{futureValue.toLocaleString('en-US', {
									minimumFractionDigits: 2,
									maximumFractionDigits: 2
								})}</span
							>
						</div>
					</div>
					<Button class="w-full !py-3 text-base">Start Saving</Button>
				</div>
				<div>
					<ResponsiveContainer width="100%" height={200}>
						<PieChart>
							<Pie
								data={savingsPieData}
								dataKey="value"
								nameKey="name"
								cx="50%"
								cy="50%"
								innerRadius={50}
								outerRadius={70}
								paddingAngle={5}
								labelLine={false}
								label={({ name, percent }) => `${(percent * 100).toFixed(0)}%`}
							>
								<Cell fill="var(--brand-primary)" />
								<Cell fill="var(--brand-accent)" />
							</Pie>
							<Tooltip />
							<Legend iconType="circle" />
						</PieChart>
					</ResponsiveContainer>
					<ResponsiveContainer width="100%" height={180} class="mt-4">
						<LineChart data={savingsGrowthData} margin={{ top: 5, right: 20, left: -10, bottom: 5 }}>
							<CartesianGrid strokeDasharray="3 3" />
							<XAxis dataKey="month" label={{ value: 'Months', position: 'insideBottom', offset: -5 }} />
							<YAxis tickFormatter={(val) => `₦${Number(val / 1000).toFixed(0)}k`} />
							<Tooltip formatter={(value: number) => `₦${value.toLocaleString('en-US', { maximumFractionDigits: 0 })}`} />
							<Line type="monotone" dataKey="value" stroke="var(--brand-primary)" strokeWidth={2} dot={false} />
						</LineChart>
					</ResponsiveContainer>
				</div>
			</div>
		{/if}
	</div>
</Card>
