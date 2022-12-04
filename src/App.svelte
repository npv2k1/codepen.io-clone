<script>
  import { onMount } from "svelte";

  // import "./lib/codemirror.css";
  import { HSplitPane, VSplitPane } from "svelte-split-pane";
  import { loop_guard } from "svelte/internal";
  import { makePage } from "./build";
  import { Language } from "./common/enums";
  import Editor from "./Editor.svelte";
  import "./userWorker.ts";

  let languageSelect = Language.HTML;

  let htmlEditor;
  let cssEditor;
  let jsEditor;

  let output = makePage("<h1>Hello</h1>");

  $: {
    console.log(htmlEditor?.getValue());
  
  }

  // // console.log(output);
  const handleClick = () => {
    console.log(htmlEditor);
    console.log("htmlEditor", htmlEditor.getValue());
  };

  onMount(() => {
    setInterval(() => {
      output = makePage(
        htmlEditor?.getValue(),
        cssEditor?.getValue(),
        jsEditor?.getValue()
      );
    }, 1000);
  });
  let paneRightW;
  let paneRightH;

  $: {
    console.log({ paneRight: paneRightW, paneRightH });
    console.log();
  }

  const handleSaveFile = () => {};
</script>

<main>
  <div class="wrapper flex flex-col">
    <div class="p-2">
      <!-- save -->
      <button
        class=" bg-blue-300 rounded-xl px-2 py-1 mx-3 border hover:border-blue-700 border-transparent"
        on:click={handleSaveFile}>Save</button
      >
    </div>
    <div class="flex flex-1">
      <div class="flex flex-1">
        <div class="flex flex-1 flex-col">
          <div>
            <ul
              class="hidden text-sm font-medium text-center text-gray-500 rounded-lg divide-x divide-gray-200 shadow sm:flex dark:divide-gray-700 dark:text-gray-400"
            >
              <li
                on:click={() => (languageSelect = Language.HTML)}
                class="w-full"
              >
                <a
                  href="#"
                  class={`inline-block p-4 w-full text-gray-900 bg-gray-100  " ${
                    languageSelect === Language.HTML
                      ? " border-t-2 border-blue-500"
                      : ""
                  }`}
                  aria-current="page">HTML</a
                >
              </li>
              <li
                on:click={() => (languageSelect = Language.CSS)}
                class="w-full"
              >
                <a
                  href="#"
                  class={`inline-block p-4 w-full text-gray-900 bg-gray-100  " ${
                    languageSelect === Language.CSS
                      ? "border-t-2 border-blue-500"
                      : ""
                  }`}>CSS</a
                >
              </li>
              <li
                on:click={() => (languageSelect = Language.JS)}
                class="w-full"
              >
                <a
                  href="#"
                  class={`inline-block p-4 w-full text-gray-900 bg-gray-100   " ${
                    languageSelect === Language.JS
                      ? "border-t-2 border-blue-500"
                      : ""
                  }`}>javascript</a
                >
              </li>
            </ul>
          </div>
          {#if languageSelect === Language.HTML}
            <div class="w-full h-full">
              <Editor language="html" bind:editor={htmlEditor} />
            </div>
          {:else if languageSelect === Language.CSS}
            <div class="w-full h-full">
              <Editor language="css" bind:editor={cssEditor} />
            </div>
          {:else if languageSelect === Language.JS}
            <div class="w-full h-full">
              <Editor language="javascript" bind:editor={jsEditor} />
            </div>
          {/if}
        </div>
      </div>
      <div
        bind:clientWidth={paneRightW}
        bind:clientHeight={paneRightH}
        class="bg-gray-200 flex flex-[2]"
      >
        <iframe title="code" srcDoc={output} class="w-full h-full" />
      </div>
    </div>
  </div>
</main>

<style>
  main {
    margin: 0 auto;
  }
  div.wrapper {
    width: 100vw;
    height: 100vh;
    margin: auto;
  }
  left,
  right,
  top,
  down {
    width: 100%;
    height: 100%;
    display: flex;
    /* display: block; */
  }
</style>
