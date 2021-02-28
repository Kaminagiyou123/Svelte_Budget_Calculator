<script>
	//components
	import Navbar from './Navbar.svelte'
	import ExpenseList from './ExpenseList.svelte'
	import expensesData from './expenses'
	// // variables
	let expenses=[...expensesData]
</script>
<!-- <style></style> -->
<!-- CSS/STYLING -->
<Navbar/>

<main class='content'>
	<ExpenseList {expenses}/>
</main>
	

