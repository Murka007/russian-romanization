<script lang="ts">
    import { onMount } from "svelte";

    let open = false;
    export let selected = 2;
    const options = ["Light", "Medium", "Advanced"] as const;

    const choose = (option: number) => {
        selected = option;
        open = false;
    }

    let dropdown: HTMLDivElement | null = null;
    const handleClick = (event: PointerEvent) => {
        if (dropdown !== null && !dropdown.contains(event.target as HTMLElement)) {
            open = false;
        }
    }

    onMount(() => {
        document.addEventListener("click", handleClick);
    });

</script>

<div bind:this={dropdown} class="relative w-40 cursor-pointer select-none">
    <button
        class="duration-200 ease-out hover:brightness-80 cursor-pointer w-full px-3 py-1 text-sm text-left rounded-lg border border-gray-300 shadow-sm focus:outline-none"
        onclick={() => (open = !open)}
    >
        {options[selected]}
        <span class="float-right">▾</span>
    </button>

    {#if open}
        <ul
            class="absolute w-full mt-1 border border-gray-300 rounded-lg shadow-md z-10"
        >
            {#each options as option, i}
                <li>
                    <button
                        type="button"
                        class="cursor-pointer w-full text-sm text-left px-3 py-2 hover:bg-black/5 dark:hover:bg-white/10"
                        onclick={() => choose(i)}
                    >
                        {option}
                    </button>
                </li>
            {/each}
        </ul>
    {/if}

</div>
