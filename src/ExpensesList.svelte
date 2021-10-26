<script>
    import SectionTitle from "./Title.svelte";
    import Expense from "./Expense.svelte";
    export let expenses = [];
</script>

<section>
    <SectionTitle title="expense list" />
    <ul>
        {#each expenses as expense, index}
            <Expense {...expense} index={index + 1} />
        {:else}
            <h2>항목이 없습니다!</h2>
        {/each}
    </ul>
</section>
