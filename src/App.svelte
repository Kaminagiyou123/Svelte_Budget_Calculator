<script>
	//components
	import Navbar from './Navbar.svelte'
	import ExpenseList from './ExpenseList.svelte'
	import Modal from './Modal.svelte'
	import Totals from './Totals.svelte'
	import { setContext,onMount,afterUpdate} from 'svelte';
	import ExpenseForm from './ExpenseForm.svelte'
	// // variables & functions
	let expenses=[];
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
		}
		)
		setId=null;
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
	//local storage
	const setLocalStorage=()=>{
		localStorage.setItem('expenses',JSON.stringify(expenses))
	}
	onMount(()=>{
		expenses= localStorage.getItem('expenses')?
		JSON.parse(localStorage.getItem('expenses')):[]
	})
	afterUpdate(()=>{
		setLocalStorage()
	})
</script>
<!-- <style></style> -->
<!-- CSS/STYLING -->
<Navbar {showForm}/>
<main class='content'>
	{#if isFormOpen}
	<Modal>
	<ExpenseForm {addExpense} 
	name={setName} amount={setAmount}
	isEditing={isEditing} {editExpense}
	{hideForm}/></Modal>
	{/if}
	<Totals title='total expense' total={total}/>
	<ExpenseList {expenses}/>
	<buttom type='button' class='btn btn-primary btn-block' on:click={clearExpenses}>Clear Expenses</buttom>
</main>

	

