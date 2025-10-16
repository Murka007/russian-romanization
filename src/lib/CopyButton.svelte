<script lang="ts">
    import { tick } from "svelte";
    import { fade } from "svelte/transition";
    export let text = "";
    const duration = 1500;

    let copied = false;
    let timeoutID: ReturnType<typeof setTimeout> | null = null;

    const handleCopy = async () => {
        if (!text) return;
        await navigator.clipboard?.writeText(text);
        copied = true;
        await tick();

        if (timeoutID !== null) clearTimeout(timeoutID);
        timeoutID = setTimeout(() => (copied = false), duration);
    }
</script>

<div class="relative inline-block">
    {#if copied}
        <div
            class="select-none absolute -top-9 left-1/2 -translate-x-1/2 px-2 py-1 font-medium text-xs rounded-md bg-green-300 text-gray-800 shadow-md"
            transition:fade={{ duration: 75 }}
        >
            Copied!
        </div>
    {/if}

    <button
        class="duration-200 ease-out px-3 py-1 rounded-lg border border-gray-300 shadow-sm text-sm hover:brightness-80 cursor-pointer"
        on:click={handleCopy}
    >
        Copy
    </button>
</div>