<script>
    import Title from './Title.svelte';
    import Expense from './Expense.svelte';
    import { blur,fly,fade } from 'svelte/transition';
    import { flip } from 'svelte/animate';
    export let expenses=[];
</script>
<style>
    h2 {
        text-transform: capitalize;
    }
</style>

<section>
    <Title title='expense list'/>
    <ul>
        {#each expenses as expense,index (expense.id)
        }
        <div transition:fly={{x:200, delay:(index)*700 
        }}  animate:flip>
        <Expense {...expense} />
        </div>
        {:else}
        <h2>no expenses added to the list</h2>
	{/each}
    </ul>
</section>