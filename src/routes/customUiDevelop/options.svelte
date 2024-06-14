<!-- Title -->
<div class="companyName">
    <i class="ri-brain-2-fill titleIcon"></i>
    <p class="title">YuukiAsunaAI</p>
</div>
<!-- Multiple Options -->
<div class="list-functions">
    <div class="functions" id="aiChat">
        <a href="/mainPage/aiChat">
            <i class="ri-chat-1-line functions-icon"></i>
            <p class="functions-text">AI Chat Helper</p>
        </a>
    </div>
    <div class="functions">
        <i class="ri-file-3-line functions-icon"></i>
        <p class="functions-text">About AI</p>
    </div>
    <div class="functions">
        <i class="ri-bar-chart-box-line functions-icon"></i>
        <p class="functions-text">Statistics</p>
    </div>

    <div class="functions">
        <i class="ri-question-line functions-icon"></i>
        <p class="functions-text">Updates & FAQ</p>
    </div>
    <div class="functions">
        <i class="ri-settings-3-line functions-icon"></i>
        <p class="functions-text">Settings</p>
    </div>
</div>

<style>
    .companyName {
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 5px;
        margin-left: 20%;
        width: 100%;
        height: 100px;
    }
    .title {
        font-size: 35px;
        margin-right: 50%;
        color: white;
    }
    .titleIcon {
        font-size: 35px;
        color: pink;
    }

    .list-functions {
        display: flex;
        align-items: center;
        flex-direction: column;
        height: 100%;
        width: 100%;
        gap: 20px;
        padding-top: 50px;
    }
    .functions {
        display: flex;
        width: 75%;
        height: 50px;
        border-radius: 15px;
        align-items: center;
        gap: 10px;
        padding: 15px;
        /* background-color: rgba(214, 202, 202, 0.1); */
        padding-left: 30px;
        cursor: pointer;
    }
    .functions-icon {
        font-size: 19px;
        color: gray;
    }
    .functions-text {
        font-size: 19px;
        color: white;
    }

    a {
        display: flex;
        gap: 10px;
    }
</style>
