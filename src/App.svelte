<script>
	import { setContext, onMount, afterUpdate } from "svelte";
	import Github from "./Github.svelte";
	import GithubAwait from "./GithubAwait.svelte";

	const state = {
		name: "simple name",
		remove: removeExpense,
	};
	// components
	import Navbar from "./Navbar.svelte";
	import ExpensesList from "./ExpensesList.svelte";
	import Totals from "./Totals.svelte";
	import ExpenseForm from "./ExpenseForm.svelte";
	import Modal from "./Modal.svelte";
	// data
	import expressData from "./expenses";

	// variables
	let expenses = [];

	// set editing variables
	let setName = "";
	let setAmount = null;
	let setId = null;

	// toggle form variables
	let isFormOpen = false;
	// reactive
	$: isEditing = setId ? true : false;
	$: total = expenses.reduce((acc, curr) => {
		return (acc += curr.amount);
	}, 0);

	// functions
	function showForm() {
		isFormOpen = true;
	}

	function hideForm() {
		isFormOpen = false;
		setName = "";
		setAmount = null;
		setId = null;
	}

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

	function setModifiedExpense(id) {
		let expense = expenses.find((item) => item.id === id);
		setId = expense.id;
		setName = expense.name;
		setAmount = expense.amount;
		showForm();
	}

	function editExpense({ name, amount }) {
		expenses = expenses.map((item) => {
			return item.id === setId
				? { ...item, name: name, amount: amount }
				: { ...item };
		});

		setId = null;
		setAmount = null;
		setName = "";
	}

	// context
	setContext("remove", removeExpense);
	setContext("modify", setModifiedExpense);

	// local storage
	function setLocalStorage() {
		localStorage.setItem("expenses", JSON.stringify(expenses));
	}

	onMount(() => {
		expenses = localStorage.getItem("expenses")
			? JSON.parse(localStorage.getItem("expenses"))
			: [];
	});

	afterUpdate(() => {
		console.log("after update");
		setLocalStorage();
	});
</script>

<Navbar {showForm} />
<main class="content">
	<!-- <GithubAwait /> -->
	{#if isFormOpen}
		<Modal>
			<ExpenseForm
				{addExpense}
				name={setName}
				amount={setAmount}
				{isEditing}
				{editExpense}
				{hideForm}
			/>
		</Modal>
	{/if}
	<Totals title="totlaal expenses" {total} />
	<ExpensesList {expenses} />
	<button
		on:click={clearExpenses}
		type="button"
		class="btn btn-primary btn-block">clear expenses</button
	>
</main>

<style></style>
