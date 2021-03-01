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
	let setName='';
	let setAmount=null;
	let setId=null;
	//toggle variables
	let isFormOpen=false;
	//functions
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
	const showForm=()=>{
		isFormOpen=true
	}
	const hideForm=()=>{
		isFormOpen=false
		setName=''
		setAmount=null;
		setId=null
	}
	const setModifiedExpense=(id)=>{
		let expense=expenses.find(item=>item.id===id)
		setId=expense.id;
		setName=expense.name;
		setAmount=expense.amount;
		showForm()
	}
	const editExpense=({name,amount})=>{
		expenses=expenses.map((item)=>{
			if (item.id===setId){
				return {...item,name,amount}
			} else {
				return {...item}
			}
		})
		sestId=null;
		setAmount=null;
		setName=''
	}


	//reactive
	$: total=expenses.reduce((acc,curr)=>{
		return (acc+=curr.amount)
	},0)
	$: isEditing = setId? true:false;

	// setContext
	setContext('remove',removeExpense)
	setContext('modify',setModifiedExpense)
</script>
<!-- <style></style> -->
<!-- CSS/STYLING -->
<Navbar {showForm}/>
<main class='content'>
	{#if isFormOpen}
	<ExpenseForm {addExpense} 
	name={setName} amount={setAmount}
	isEditing={isEditing} {editExpense}
	{hideForm}/>
	{/if}
	<Totals title='total expense' total={total}/>
	<ExpenseList {expenses}/>
	<buttom type='button' class='btn btn-primary btn-block' on:click={clearExpenses}>Clear Expenses</buttom>
</main>
	

