<script>
    import "../src/assets/global.css";
    import StrongholdCalc from "./lib/StrongholdCalc.svelte";
</script>

<main class="container">
    <StrongholdCalc />
</main>

<style>
    .container {
        display: flex;
        height: 100vh;
        flex-direction: column;
        justify-content: center;
        padding: 0 1em;
    }
</style>
