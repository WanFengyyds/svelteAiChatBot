<script>
    import Options from "../customUiDevelop/options.svelte";
</script>

<div class="page">
    <!-- 
        Left part of the page with title ecc.
    -->
    <div class="options">
        <Options></Options>
    </div>
    <div class="actualChat">
        <slot></slot>
    </div>
</div>

<style>
    .page {
        height: 100vh;
        display: flex;
        flex-direction: row;
        font-family: "Outfit", sans-serif;
        font-optical-sizing: auto;
        font-style: normal;
    }

    .options {
        flex: 0.2;
        display: flex;
        flex-direction: column;
        background-color: rgb(26, 25, 25);
    }

    .actualChat {
        display: flex;
        flex: 0.8;
        flex-direction: column;
        justify-content: center;
        height: 100vh;
        background-color: rgb(32, 31, 31);
    }
</style>
