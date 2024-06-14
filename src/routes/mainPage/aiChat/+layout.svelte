<script>
    import { Input } from "$lib/components/ui/input/index.js";
    import { ModeWatcher } from "mode-watcher";
    import { onMount } from "svelte";
    import { Messaggio } from "../../../lib/models/Messaggio.js";

    let token = "Bearer ";
    let chat = "";
    let arrayOfMessages = [];

    function removeSubstring(str) {
        if (str == "event: end") {
            return "";
        }
        return str.split("data:").join("");
    }

    function extractAccessTokenFromString(dataStr) {
        try {
            // Parse the JSON string into an object
            const data = JSON.parse(dataStr);

            // Check if the accessToken exists and return it in an object
            if (data && data.accessToken) {
                return { accessToken: data.accessToken };
            } else {
                return {};
            }
        } catch (error) {
            console.error("Invalid JSON string:", error);
            return {};
        }
    }
    onMount(() => {
        token += extractAccessTokenFromString(
            localStorage.getItem("key"),
        ).accessToken;
    });

    async function handleEnterPress(event) {
        if (event.key === "Enter" && chat != "") {
            arrayOfMessages.push(new Messaggio({ testo: chat, from: "me" }));
            arrayOfMessages = [...arrayOfMessages];
            const myHeaders = new Headers();
            myHeaders.append("accept", "*/*");
            myHeaders.append("Content-Type", "application/json");
            myHeaders.append("Authorization", token);

            const raw = JSON.stringify({
                messaggio: chat,
            });

            chat = "";
            const requestOptions = {
                method: "POST",
                headers: myHeaders,
                body: raw,
                redirect: "follow",
            };

            try {
                const response = await fetch(
                    "http://localhost:5257/ollama",
                    requestOptions,
                );

                if (!response.ok) {
                    throw new Error("Network response was not ok");
                }

                const reader = response.body.getReader();
                const decoder = new TextDecoder("utf-8");
                let result = "";
                arrayOfMessages.push(new Messaggio({ testo: "", from: "bot" }));

                while (true) {
                    const { value, done } = await reader.read();
                    if (done) break;

                    result += decoder.decode(value, { stream: true });
                    console.log(result);
                    arrayOfMessages[arrayOfMessages.length - 1].testo =
                        removeSubstring(result);
                    arrayOfMessages = [...arrayOfMessages];
                    console.log(result);
                }

                console.log(result);

                arrayOfMessages = [...arrayOfMessages];
            } catch (error) {
                console.error(error);
            }
        }
    }
</script>

<ModeWatcher />
<slot></slot>
<div class="all">
    <div class="containerChat">
        <div class="chatPart">
            <div class="topBar">
                <p class="chatTitle">AI Chat Helper</p>
            </div>

            <div class="messages">
                {#each arrayOfMessages as messaggio}
                    {#if messaggio.from == "bot"}
                        <div class="messageFromBot">
                            <div class="message">
                                <p class="chat">{messaggio.testo}</p>
                            </div>
                        </div>
                    {:else}
                        <div class="messageFromUser">
                            <div></div>
                            <div class="message">
                                <p class="chatBot">{messaggio.testo}</p>
                            </div>
                        </div>
                    {/if}
                {/each}
            </div>
            <div class="input">
                <Input
                    type="email"
                    placeholder="Type ur question"
                    class="inputChat"
                    on:keypress={handleEnterPress}
                    bind:value={chat}
                />
            </div>
        </div>
        <div class="chatHistory">
            <div class="history-box">
                <p class="chatTitle">History</p>
            </div>
            <div class="all-history-container"></div>
        </div>
    </div>
</div>

<style>
    .all {
        display: flex;
        height: 100%;
        width: 100%;
        justify-content: center;
        align-items: center;
    }
    .containerChat {
        display: flex;
        flex-direction: row;
        height: 95%;
        width: 95%;
        border-radius: 20px;
        background-color: white;
    }
    .chatPart {
        display: flex;
        flex-direction: column;
        height: 100%;
        flex: 0.85;
    }
    .chatHistory {
        display: flex;
        height: 100%;
        flex: 0.15;
        flex-direction: column;
    }
    .history-box {
        display: flex;
        padding: 5px;
        height: 50px;
        border-left: 2px solid lightgray;
        border-bottom: 2px solid lightgray;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        width: 100%;
    }

    .all-history-container {
        display: flex;
        width: 100%;
        height: 95%;
        border-left: 2px solid lightgray;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    }

    .topBar {
        display: flex;
        flex-direction: row;
        height: 50px;
        width: 100%;
        align-items: center;
        border-radius: 20px 20px 0 0;
        background-color: white;
        padding: 10px;
        border-bottom: 2px solid lightgray;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        padding-left: 20px;
    }

    .chatTitle {
        font-family: "Outfit", sans-serif;
        font-optical-sizing: auto;
        font-style: normal;
        font-size: 25px;
        color: black;
    }

    .messages {
        display: flex;
        flex-direction: column;
        height: 100%;
        width: 100%;
        gap: 40px;
        /*overflow-y: scroll;*/
    }
    .input {
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
        padding-left: 10%;
        padding-right: 10%;
        padding-bottom: 30px;
    }

    .messageFromBot {
        display: flex;
        width: 100%;
        height: fit-content;
        align-items: center;
        justify-content: start;
        padding-left: 30px;
    }
    .messageFromUser {
        display: flex;
        width: 100%;
        height: fit-content;
        align-items: center;
        flex-direction: row;
        justify-content: space-between;
        padding-right: 30px;
    }

    .message {
        display: flex;
        align-items: center;
        padding: 5px;
        background-color: rgb(211, 211, 211, 0.5);
        border-radius: 15px;
        max-width: 70%;
    }
    .chat {
        font-size: 20px;
    }
    .chatBot {
        font-size: 20px;
    }
</style>
