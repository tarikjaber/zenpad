<script lang="ts">
    import { onMount } from "svelte";

    // Handle textarea input
    function handleTextareaInput(event: any) {
        const newValue = event.target.value;
        localStorage.setItem("text", newValue);
    }

    // Handle Tab key press
    function handleTabPress(event: KeyboardEvent) {
        if (event.key === "Tab") {
            event.preventDefault();
            const textarea = event.target as HTMLTextAreaElement;
            const { selectionStart, selectionEnd } = textarea;
            const value = textarea.value;
            const newValue = value.substring(0, selectionStart) + "    " + value.substring(selectionEnd);
            textarea.value = newValue;
            // Set the cursor position after the inserted spaces
            textarea.selectionStart = textarea.selectionEnd = selectionStart + 4;
        }
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

        // Event listener to handle Tab key press
        textarea?.addEventListener("keydown", handleTabPress);
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
        caret-color: #DAFFFB; /* Set the caret color to match the scrollbar */
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
        background-color: #176B87; /* Set the selected text background color to match the scrollbar on hover */
        color: #DAFFFB; /* Set the selected text color */
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
        background-color: #001C30;
        color: var(--text-color);
    }
</style>
