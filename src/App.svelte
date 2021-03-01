<script>
	//components
	import Navbar from './Navbar.svelte'
	import ExpenseList from './ExpenseList.svelte'
	import expensesData from './expenses'
	import Totals from './Totals.svelte'
	import { setContext } from 'svelte';
	import ExpenseForm from './ExpenseForm.svelte'
	// // variables & functions
	let expenses=[...expensesData]
	const removeExpense=(id)=>{
		expenses=expenses.filter((expense)=>expense.id!==id);
	}
	const clearExpenses=()=>{
		expenses=[]
	}
	const addExpense=({name,amount})=>{
		let expense={id:Math.random()*Date.now(),name,amount}
		expenses=[expense,...expenses]
        
    }
	//reactive
	$: total=expenses.reduce((acc,curr)=>{
		return (acc+=curr.amount)
	},0)
	
	// setContext
	setContext('remove',removeExpense)
</script>
<!-- <style></style> -->
<!-- CSS/STYLING -->
<Navbar/>
<main class='content'>
	<ExpenseForm {addExpense}/>
	<Totals title='total expense' total={total}/>
	<ExpenseList {expenses}/>
	<buttom type='button' class='btn btn-primary btn-block' on:click={clearExpenses}>Clear Expenses</buttom>
</main>
	

