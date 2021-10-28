<script>
    import SectionTitle from "./Title.svelte";
    import Expense from "./Expense.svelte";
    import { fly } from "svelte/transition";
    import { flip } from "svelte/animate";

    export let expenses = [];
</script>

<section>
    <SectionTitle title="expense list" />
    <ul>
        {#each expenses as expense, index (expense.id)}
            <div
                in:fly={{ x: 200, delay: (index + 1) * 900 }}
                out:fly={{ x: -200 }}
                animate:flip
            >
                <Expense {...expense} />
            </div>
        {:else}
            <h2>항목이 없습니다!</h2>
        {/each}
    </ul>
</section>
