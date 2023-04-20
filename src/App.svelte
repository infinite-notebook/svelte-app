<script lang="ts">
  import Sidebar from './Sidebar.svelte'
  import Editor from './Editor.svelte'
  import SaveButton from './SaveButton.svelte'
  import * as Y from 'yjs'
  import { WebrtcProvider } from 'y-provider'

  let pw = '1299651405'
 
  const ydoc = new Y.Doc()
  const provider = new WebrtcProvider('1299651405', ydoc, { password: pw })
  const body = ydoc.getXmlFragment()
  const title = ydoc.getXmlFragment('title')
  let sidebar = false
</script>

<main
  style="display: flex; flex-direction: row; width: inherit; height: inherit"
>
  <div class="Sidebar" style={sidebar ? 'width: 300px' : 'width: 0px'}>
    <Sidebar yXmlFragment={body} />
  </div>
  <div class="Separator" on:click={() => {sidebar = !sidebar}} />

  <div class="EditorContainer">
    <SaveButton ydoc={ydoc}/>
    <div class="Editor">
      <Editor {body} {provider} {title} />
    </div>
  </div>
</main>

<style>
  .Sidebar {
    color: #333;
    background-color: #eee;
  }

  .Separator {
    width: 10px;
    background-color: #9e9e9e;
  }
  .Sidebar {
    transition: width 0.3s;

    box-sizing: border-box;
    z-index: 11;
    /* width: 30rem; */
    overflow-x: hidden;
    white-space: nowrap;

    overflow-y: auto;
  }

  .EditorContainer {
    width: 100%;
    overflow-x: hidden;
    overflow-y: auto;
  }

  .Editor {
    min-width: 320px;
    max-width: 75ch;
    margin: 0 auto;
  }
  @media (prefers-color-scheme: dark) {
    .Header,
    .Sidebar {
      color: #eee;
      background-color: #222;
      border-color: #555;
      outline-color: #555;
    }
  }
</style>
