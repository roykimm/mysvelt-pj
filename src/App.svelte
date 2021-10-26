<script>
	import { setContext } from "svelte";
	const state = {
		name: "simple name",
		remove: removeExpense,
	};
	// components
	import Navbar from "./Navbar.svelte";
	import ExpensesList from "./ExpensesList.svelte";
	import Totals from "./Totals.svelte";
	import ExpenseForm from "./ExpenseForm.svelte";
	// data
	import expressData from "./expenses";

	// variables
	let expenses = [...expressData];

	// reactive
	$: total = expenses.reduce((acc, curr) => {
		return (acc += curr.amount);
	}, 0);

	// functions
	function removeExpense(id) {
		expenses = expenses.filter((item) => item.id !== id);
	}

	function clearExpenses() {
		expenses = [];
	}

	function addExpense({ name, amount }) {
		console.log(name, amount);
		let expense = {
			id: Math.random() * Date.now(),
			name: name,
			amount: amount,
		};
		expenses = [expense, ...expenses];
	}

	// context
	setContext("remove", removeExpense);
</script>

<Navbar />
<main class="content">
	<ExpenseForm {addExpense} />
	<Totals title="totlaal expenses" {total} />
	<ExpensesList {expenses} />
	<button
		on:click={clearExpenses}
		type="button"
		class="btn btn-primary btn-block">clear expenses</button
	>
</main>

<style></style>
