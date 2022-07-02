<script lang="ts">
  import * as monaco from "monaco-editor/esm/vs/editor/editor.api";
  import { afterUpdate, onMount } from "svelte";
  import CodeMirror from "codemirror";
  export let language: string = "plaintext";

  let monacoEl;
  export let editor;
  $: {
    console.log(editor?.getValue());
  }

  onMount(() => {
    if (!monacoEl) return;
    editor = monaco.editor.create(monacoEl, {
      value: '',
      language: language,
      theme: "vs-dark",
      minimap: {
        enabled: false
      },
      automaticLayout: true 
    });
      // editor = CodeMirror.fromTextArea(monacoEl, {
      //   value: "function myScript(){return 100;}\n",
      //   lineNumbers: true,
      //   theme: "dracula",
      //   mode: "javascript"
      // });
      // editor.setSize("100%", "100%");
      // editor.on('change', () => {
      //   console.log(editor.getValue());
      // });
    
  });
</script>

<div class="w-full h-full" bind:this={monacoEl} />
