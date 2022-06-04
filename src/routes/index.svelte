<script context="module">
	export const prerender = true;
</script>

<script>
	/*
	Functions for reactive realistic interest rate

	features
	count realistic interest date based on exact calendar instead of 360 days a year.
	
*/

	import { Datepicker } from 'svelte-calendar';
	import dayjs from 'dayjs';
	
	var startDate;
	var endDate;
	$: dateEnd = endDate;
	// loan has to start from today
	const startingFromDateStart = dayjs();
	// until +1 year from current date
	const startingFromDateEnd = dayjs().add(1, 'year');
	// lending time in days
	let lendingTime = 360;

	var date1 = dayjs();
	var date2 = dayjs().add(lendingTime, 'days');
	$: date1react = dayjs();
	$: date2 = dayjs().add(lendingTime, 'days');
	// lending amount in dollars
	let lendingAmount = 10000;
	// lending rate in percents
	let lendingRate = 3.875;
	// count lending years between dates
	var lendingYears;
	$: lendingYears = date1react.add(lendingTime, 'days').diff(date1, 'years', true);

	// lets calculate total
	$: totalAccruedAmountOrdinary = lendingAmount * (1 + (lendingRate / 100) * (lendingTime / 360));
	$: totalAccruedAmountExact =
		lendingAmount *
		(1 + (lendingRate / 100) * date1react.add(lendingTime, 'days').diff(date1, 'years', true));

	const theme = {
		calendar: {
			width: '400px',
			shadow: '0px 0px 5px rgba(0, 0, 0, 0.25)'
		}
	};
</script>

<svelte:head>
	<title>Interest calculator</title>
	<meta name="description" content="Interest calculator" />
</svelte:head>

<section>
	date1:{date1react.format('MM/DD/YYYY')}<br />
	date2:{date2.format('MM/DD/YYYY')}<br />

	<div>
		<h1>Exact: {totalAccruedAmountExact}</h1>
		{totalAccruedAmountExact} = {lendingAmount}(1 + ({lendingRate}/100)*{lendingYears} )
		<br />
		A = P(1 + rt)
	</div>
	<div>
		<h1>Ordinary 360: {totalAccruedAmountOrdinary}</h1>
		{totalAccruedAmountOrdinary} = {lendingAmount}(1 + ({lendingRate}/100)*{lendingTime}/360 )
		<p>A = P(1 + rt)</p>
	</div>
	<div>
		<p>Number of days to pay loan back</p>
		<label>
			<input type="number" bind:value={lendingTime} min="1" max="10000" />
			<input type="range" bind:value={lendingTime} min="1" max="10000" />
		</label>
	</div>

	Rate(%):<input bind:value={lendingRate} /><br />
	Start date(not working - starts today atm):
	<Datepicker bind:date1 start={startingFromDateStart} end={startingFromDateEnd} {theme} />
	<br />
	End date:
	<input bind:value={date2} readonly />
	<br />
	Where:<br />
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
