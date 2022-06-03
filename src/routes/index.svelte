<script context="module">
	export const prerender = true;
</script>

<script>
	import { Datepicker } from 'svelte-calendar';
	import dayjs from 'dayjs';
	import duration from 'dayjs/plugin/duration' // import plugins
	dayjs.extend(duration) // use plugin

	// loan has to start from today
	const startingFromDateStart = dayjs();
	// until +1 year from current date
	const startingFromDateEnd = dayjs().add(1, 'year');
	// lending time in days
	let lendingTime = 1;
	
	var date1 = dayjs();
	var date2 = dayjs().add(lendingTime, 'days');
	$: date2 = dayjs().add(lendingTime, 'days');
	var lendingYears = date2.diff(date1, 'years', true);
	
	// lending amount in dollars
	let lendingAmount = 10000;
	// lending rate in percents
	let lendingRate = 3.875;
	// lets calculate total
	let totalAccruedAmount360 = lendingAmount*(1+(lendingRate/100)*(lendingTime/360));
	$: totalAccruedAmount360 = lendingAmount*(1+(lendingRate/100)*(lendingTime/360));
	let totalAccruedAmountExact = lendingAmount*(1+(lendingRate/100)*lendingYears);
	$: totalAccruedAmountExact = lendingAmount*(1+(lendingRate/100)*lendingYears);
	
	const theme = {
		calendar: {
			width: '400px',
			shadow: '0px 0px 5px rgba(0, 0, 0, 0.25)'
		}
	};

	let startDate;
	let endDate;
	$: endDate;
	
</script>

<svelte:head>
	<title>Interest calculator</title>
	<meta name="description" content="Interest calculator" />
</svelte:head>

<section>
	{date1} {date2} = {lendingYears}
	<h1>Exact: {totalAccruedAmountExact}<br> vs.<br> 360: {totalAccruedAmount360}</h1>
	<br>
		<p>Number of days to pay loan back</p>
	<label>
		<input type=number bind:value={lendingTime} min=1 max=10000>
		<input type=range bind:value={lendingTime} min=1 max=10000>
	</label>
	
	Rate(%): <input style="display:inline-block" bind:value={lendingRate}><br>
	Start date:
	<Datepicker bind:startDate start={startingFromDateStart} end={startingFromDateEnd} {theme} />
	<br>
	End date:
	<input bind:value={endDate} readonly/>
		<br>
	<h3>exact interest calculation:</h3> <br>
	{totalAccruedAmountExact} = {lendingAmount}(1 + ({lendingRate}/100)*{lendingTime} )
	<br>
	A = P(1 + rt)<br><br>
	<br>
	<h3>ordinary interest calculation:</h3> <br>
	{totalAccruedAmount360} = {lendingAmount}(1 + ({lendingRate}/100)*{lendingTime}/360 )
	<br>
	A = P(1 + rt)<br><br>
	Where:<br>
	<ul>
		<li>A = Total Accrued Amount (principal + interest)</li>
		<li>P = Principal Amount</li>
		<li>I = Interest Amount</li>
		<li>r = Rate of Interest per year in decimal; r = R/100</li>
		<li>R = Rate of Interest per year as a percent; R = r * 100</li>
		<li>t = Time Period involved in years</li>
	</ul>
</section>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 1;
	}

</style>
