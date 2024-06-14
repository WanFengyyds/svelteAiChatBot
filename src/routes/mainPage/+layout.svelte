<script>
    import ChatHistory from "../customUiDevelop/chatHistory.svelte";
</script>

<div class="page">
    <!-- 
        Left part of the page with title ecc.
    -->
    <div class="options">
        <ChatHistory></ChatHistory>
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
        padding-bottom: 50px;
        background-color: rgb(32, 31, 31);
    }
</style>
