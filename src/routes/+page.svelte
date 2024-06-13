<script lang="ts">
    import * as Tabs from "$lib/components/ui/tabs/index.js";
    import * as Card from "$lib/components/ui/card/index.js";
    import { Button } from "$lib/components/ui/button/index.js";
    import { Input } from "$lib/components/ui/input/index.js";
    import { Label } from "$lib/components/ui/label/index.js";
    import { ModeWatcher } from "mode-watcher";
    import { browser } from "$app/environment";
    import { writable } from "svelte/store";
    import { onMount } from "svelte";

    let registerEmail = "";
    let registerPassword = "";
    let loginEmail = "";
    let loginPassword = "";
    let token = "Bearer ";

    onMount(() => {
        token += extractAccessTokenFromString(
            localStorage.getItem("key"),
        ).accessToken;

        if (token != "Bearer undefined") {
            console.log(token);
            window.location.href = "/mainPage";
        }
    });

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

    async function register() {
        const myHeaders = new Headers();
        myHeaders.append("accept", "*/*");
        myHeaders.append("Content-Type", "application/json");

        const raw = JSON.stringify({
            email: registerEmail,
            password: registerPassword,
        });

        fetch("http://localhost:5257/identity/register", {
            method: "POST",
            headers: myHeaders,
            body: raw,
            redirect: "follow",
        })
            .then((response) => response.text())
            .then((result) => console.log("Creazione con successo"))
            .catch((error) => console.error(error));
    }

    async function login() {
        const myHeaders = new Headers();
        myHeaders.append("accept", "*/*");
        myHeaders.append("Content-Type", "application/json");
        myHeaders.append("Authorization", token);

        const raw = JSON.stringify({
            email: loginEmail,
            password: loginPassword,
        });

        await fetch("http://localhost:5257/identity/login", {
            method: "POST",
            headers: myHeaders,
            body: raw,
            redirect: "follow",
        })
            .then((response) => response.text())
            .then((result) => {
                writable(browser && localStorage.setItem("key", result));
                console.log(result);
            })
            .catch((error) => console.error(error));

        token += extractAccessTokenFromString(
            localStorage.getItem("key"),
        ).accessToken;

        if (localStorage.getItem("key") != "Bearer undefined") {
            window.location.href = "/mainPage";
        }
    }
</script>

<slot />
<div class="page">
    <Tabs.Root value="account" class="w-[400px]">
        <Tabs.List class="grid w-full grid-cols-2">
            <Tabs.Trigger value="account">Login</Tabs.Trigger>
            <Tabs.Trigger value="password">Register</Tabs.Trigger>
        </Tabs.List>
        <Tabs.Content value="account">
            <Card.Root>
                <Card.Header>
                    <Card.Title>Login</Card.Title>
                    <Card.Description
                        >Login with your own account</Card.Description
                    >
                </Card.Header>
                <Card.Content class="space-y-2">
                    <div class="space-y-1">
                        <Label for="name">Email</Label>
                        <Input id="name" bind:value={loginEmail} />
                    </div>
                    <div class="space-y-1">
                        <Label for="username">Password</Label>
                        <Input id="username" bind:value={loginPassword} />
                    </div>
                </Card.Content>
                <Card.Footer>
                    <Button on:click={login}>Login</Button>
                </Card.Footer>
            </Card.Root>
        </Tabs.Content>
        <Tabs.Content value="password">
            <Card.Root>
                <Card.Header>
                    <Card.Title>Register</Card.Title>
                    <Card.Description
                        >Create your account here!</Card.Description
                    >
                </Card.Header>
                <Card.Content class="space-y-2">
                    <div class="space-y-1">
                        <Label for="current">Email</Label>
                        <Input id="current" bind:value={registerEmail} />
                    </div>
                    <div class="space-y-1">
                        <Label for="new">Password</Label>
                        <Input
                            id="new"
                            type="password"
                            bind:value={registerPassword}
                        />
                    </div>
                </Card.Content>
                <Card.Footer>
                    <Button on:click={register}>Register</Button>
                </Card.Footer>
            </Card.Root>
        </Tabs.Content>
    </Tabs.Root>
</div>

<style>
    .page {
        display: flex;
        align-items: center;
        justify-content: center;
        height: 100vh;
    }
</style>
