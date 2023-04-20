<script lang="ts">

  import './editor.css'
  import { onMount } from 'svelte'

  import { titleSchema, schema } from './schema'


  import type * as Y from 'yjs'
  import {
    ySyncPlugin,
    yCursorPlugin,
    yUndoPlugin,
    undo,
    redo,
  } from 'y-prosemirror'
  import { EditorState } from 'prosemirror-state'
  import { EditorView } from 'prosemirror-view'
  import { exampleSetup } from 'prosemirror-example-setup'
  import { keymap } from 'prosemirror-keymap'

  export let body: Y.XmlFragment
  export let provider: any
  export let title: Y.XmlFragment
  let editor: HTMLDivElement
  let titleEditor: HTMLDivElement



  onMount(() => {
    new EditorView(titleEditor, {
      state: EditorState.create({
        schema: titleSchema,
        plugins: [
          ySyncPlugin(title),
          yCursorPlugin(provider.awareness),
          yUndoPlugin(),
          keymap({
            'Mod-z': undo,
            'Mod-y': redo,
            'Mod-Shift-z': redo,
          }),
        ].concat(
          exampleSetup({
            schema: schema,
            menuBar: false,
          })
        ),
      }),
    })

    new EditorView(editor, {
      state: EditorState.create({
        schema: schema,
        plugins: [
          ySyncPlugin(body),
          yCursorPlugin(provider.awareness),
          yUndoPlugin(),
          keymap({
            'Mod-z': undo,
            'Mod-y': redo,
            'Mod-Shift-z': redo,
          }),
        ].concat(
          exampleSetup({
            schema: schema,
            menuBar: false,
          })
        ),
      }),
    })
  })
</script>

<div class="Title" bind:this={titleEditor} />
<div bind:this={editor} />

<style>
  .Title {
    text-align: center;
    font-size: 2rem;
    font-weight: bold;
    padding: 10px 10px;
  }
  div {
    padding: 0px 10px;
  }
</style>
