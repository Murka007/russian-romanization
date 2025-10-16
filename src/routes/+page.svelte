<script lang="ts">
    import CopyButton from "$lib/CopyButton.svelte";
    import Select from "$lib/Select.svelte";
    import { romanize } from "../utils/romanize";

    const initLevel = localStorage.getItem("level");
    let input = $state("");
    let level = $state(initLevel !== null ? Number(initLevel) : 2);
    let output = $derived(romanize(input, level));

    $effect(() => {
        localStorage.setItem("level", level + "");
    });
</script>

<main class="min-h-screen flex items-center justify-center p-6 select-none">

    <section class="w-full max-w-4xl flex flex-col gap-6">

        <header class="flex flex-col">
            <div class="flex items-center gap-2">
                <svg xmlns="http://www.w3.org/2000/svg" width="35" height="35" viewBox="0 0 24 24"><path fill="currentColor" fill-rule="evenodd" d="M15.958 8.95a1.5 1.5 0 0 1 1.085.02c.472.193.693.613.791.81c.112.227.223.523.337.828l.015.04l3.75 10a1 1 0 0 1-1.872.703l-3.613-9.633l-4.028 9.667a1 1 0 0 1-1.846-.77l4.179-10.028l.016-.04a10 10 0 0 1 .367-.815c.104-.194.34-.605.82-.781Zm-3.174-4.909a1 1 0 0 1 .675 1.243C12.476 8.6 11.432 11.04 9.87 13.02c-1.568 1.988-3.598 3.434-6.417 4.872a1 1 0 1 1-.909-1.782c2.683-1.369 4.434-2.654 5.756-4.329c1.329-1.685 2.285-3.842 3.24-7.064a1 1 0 0 1 1.243-.675Z" clip-rule="evenodd"/><path fill="currentColor" fill-rule="evenodd" d="M5.961 7.342a1 1 0 0 1 1.235.689c.702 2.47 2.066 4.29 3.904 5.669a1 1 0 0 1-1.2 1.6c-2.162-1.622-3.798-3.802-4.627-6.723a1 1 0 0 1 .688-1.235M2 5a1 1 0 0 1 1-1h12.5a1 1 0 1 1 0 2H3a1 1 0 0 1-1-1" clip-rule="evenodd"/><path fill="currentColor" fill-rule="evenodd" d="M9 2a1 1 0 0 1 1 1v2a1 1 0 0 1-2 0V3a1 1 0 0 1 1-1m4 15a1 1 0 0 1 1-1h5.5a1 1 0 1 1 0 2H14a1 1 0 0 1-1-1" clip-rule="evenodd"/></svg>
                <h1 class="text-2xl sm:text-3xl font-semibold tracking-tight">
                    Russian Romanization
                </h1>
                <a
                    href="https://github.com/murka007/russian-romanization/"
                    title="github"
                    class="ml-auto duration-200 ease-out hover:text-black/60 dark:hover:text-white/70"
                >
                    <svg xmlns="http://www.w3.org/2000/svg" width="30" height="30" viewBox="0 0 24 24"><g fill="none"><g clip-path="url(#SVGXv8lpc2Y)"><path fill="currentColor" fill-rule="evenodd" d="M12 0C5.37 0 0 5.37 0 12c0 5.31 3.435 9.795 8.205 11.385c.6.105.825-.255.825-.57c0-.285-.015-1.23-.015-2.235c-3.015.555-3.795-.735-4.035-1.41c-.135-.345-.72-1.41-1.23-1.695c-.42-.225-1.02-.78-.015-.795c.945-.015 1.62.87 1.845 1.23c1.08 1.815 2.805 1.305 3.495.99c.105-.78.42-1.305.765-1.605c-2.67-.3-5.46-1.335-5.46-5.925c0-1.305.465-2.385 1.23-3.225c-.12-.3-.54-1.53.12-3.18c0 0 1.005-.315 3.3 1.23c.96-.27 1.98-.405 3-.405s2.04.135 3 .405c2.295-1.56 3.3-1.23 3.3-1.23c.66 1.65.24 2.88.12 3.18c.765.84 1.23 1.905 1.23 3.225c0 4.605-2.805 5.625-5.475 5.925c.435.375.81 1.095.81 2.22c0 1.605-.015 2.895-.015 3.3c0 .315.225.69.825.57A12.02 12.02 0 0 0 24 12c0-6.63-5.37-12-12-12" clip-rule="evenodd"/></g><defs><clipPath id="SVGXv8lpc2Y"><path fill="#fff" d="M0 0h24v24H0z"/></clipPath></defs></g></svg>
                </a>
            </div>
            <p class="mt-1 text-sm opacity-80">
                Converts russian text to its equivalent in latin. Just simply type text and get a result!
            </p>
        </header>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">

            <div class="flex flex-col">
                <label for="input-text" class="mb-1 text-md">Input:</label>
                <textarea
                    id="input-text"
                    bind:value={input}
                    placeholder="Enter russian text..."
                    class="duration-200 ease-out resize-y min-h-[180px] max-h-[540px] p-4 rounded-lg shadow-md text-lg font-sans border border-gray-800 dark:border-gray-300 bg-transparent focus:outline-none focus:ring-2 focus:ring-opacity-40"
                ></textarea>

                <div class="mt-3 text-sm opacity-80">Chars: {input.length}</div>
            </div>

            <div class="flex flex-col">
                <label for="output-text" class="mb-1 text-md">Output:</label>
                <pre class="select-text whitespace-pre-wrap break-all overflow-auto h-[180px] p-4 rounded-lg shadow-md text-lg font-sans border border-gray-800 dark:border-gray-300 bg-transparent">{output}</pre>

                <div class="mt-3 flex items-center gap-3">
                    <CopyButton text={output}/>

                    <button
                        class="duration-200 ease-out px-3 py-1 rounded-lg border border-gray-300 shadow-sm text-sm hover:brightness-80 cursor-pointer"
                        onclick={() => {
                            input = "";
                        }}>Clear</button
                    >

                    <Select bind:selected={level} />
                </div>
            </div>
        </div>
        
    </section>

</main>