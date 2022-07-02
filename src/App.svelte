<script>
  import { onMount } from "svelte";

  // import "./lib/codemirror.css";
  import { HSplitPane, VSplitPane } from "svelte-split-pane";
  import { loop_guard } from "svelte/internal";
  import { makePage } from "./build";
  import Editor from "./Editor.svelte";
  import "./userWorker.ts";
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
</script>

<main>
  <div class="wrapper">
    <HSplitPane
      leftPaneSize="25%"
      rightPaneSize="75%"
      minLeftPaneSize="50px"
      minRightPaneSize="50px"
      updateCallback={(e) => {
        // htmlEditor?.layout();
        // cssEditor?.layout();
        // jsEditor?.layout();
      }}
    >
      <left slot="left">
        <VSplitPane>
          <top slot="top">
            <Editor language="html" bind:editor={htmlEditor} />
          </top>
          <down slot="down">
            <VSplitPane>
              <top slot="top">
                <Editor language="css" bind:editor={cssEditor} />
              </top>
              <down slot="down">
                <Editor language="javascript" bind:editor={jsEditor} />
              </down>
            </VSplitPane>
          </down>
        </VSplitPane>
      </left>
      <right
        bind:clientWidth={paneRightW}
        bind:clientHeight={paneRightH}
        class="bg-gray-200"
        slot="right"
      >
        <iframe
          title="code"
          srcDoc={output}
          class="w-full h-full"
        />
      </right>
    </HSplitPane>
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
