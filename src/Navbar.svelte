<script>
    export let showForm;
</script>

<nav class="nav">
    <div class="nav-center">
        <h1 class="nav-title">예산 계산기</h1>
        <button class="nav-btn" type="button" on:click={showForm}>
            <i class="far fa-plus-square" />
            항목추가
        </button>
    </div>
</nav>
