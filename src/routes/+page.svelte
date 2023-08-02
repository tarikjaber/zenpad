<script lang="ts">
    import { onMount } from "svelte";

    // Handle textarea input
    function handleTextareaInput(event: any) {
        const newValue = event.target.value;
        localStorage.setItem("text", newValue);
    }

    onMount(() => {
        const storedText = localStorage.getItem("text");

        const textarea = document.querySelector("textarea");
        if (textarea) {
            textarea.value = storedText || "";
            textarea.focus();
        }

        // Event listener to clear the textarea when "Ctrl + U" is pressed
        document.addEventListener("keydown", (event) => {
            if (event.ctrlKey && event.key === "u") {
                event.preventDefault();
                if (textarea) {
                    textarea.value = "";
                }
            }
        });
    });
</script>

<div class="main">
    <textarea class="textarea" on:input={handleTextareaInput} />
</div>

<style>
    @import url("https://fonts.googleapis.com/css2?family=Roboto&display=swap");

    .textarea {
        border: none;
        margin: 0;
        width: calc(100% - 40px); /* Adjust width to account for the scrollbar */
        max-width: 700px; /* Respect a maximum width of 500px */
        font-size: 20px;
        padding: 20px;
        /* height: calc(100vh - 40px); */
        font-family: "Roboto", sans-serif;
        resize: none;
        outline: none;
        background-color: #001C30;
        color: #DAFFFB;
        height: 100%;

        /* Curved selections */
        caret-color: #176B87; /* Set the caret color to match the scrollbar */
    }

    /* Style the scrollbar */
    .textarea::-webkit-scrollbar {
        width: 12px;
    }

    .textarea::-webkit-scrollbar-thumb {
        background-color: #176B87;
    }

    .textarea::-webkit-scrollbar-thumb:hover {
        background-color: #1c82a4;
    }

    /* Style the selected text */
    .textarea::selection {
        background-color: #1c82a4; /* Set the selected text background color to match the scrollbar on hover */
        color: #fff; /* Set the selected text color */
    }

    .main {
        display: flex;
        background-color: #001C30;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        height: 100vh;
    }

    :global(body) {
        margin: 0;
        background-color: var(--background-color);
        color: var(--text-color);
    }
</style>
