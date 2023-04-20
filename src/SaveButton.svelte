<script lang="ts">
  import * as Y from 'yjs'
  export let ydoc: Y.Doc

  let api = 'https://54hjnip2ng.execute-api.eu-north-1.amazonaws.com/'

  let status = 'unloaded'

  fetch(api)
    .then((response) => response.blob())
    .then((blob) => blob.arrayBuffer())
    .then((buffer) => new Uint8Array(buffer))
    .then((blob) => {
      Y.applyUpdate(ydoc, blob)
  })
  status = 'saved'



  const save = () => {
    status = 'saving'
    const uint8Array = new Uint8Array(Y.encodeStateAsUpdate(ydoc))
    fetch(api, {
      method: 'POST',
      body: uint8Array,
    })
      .then((response) => response.ok ? status = 'saved' : status = 'error')
  }

  ydoc.on('update', () => {
    status = 'save'
  })

</script>

<button on:click={() => save()} disabled={status != 'save' ? true : false}>{status}</button>

<style>
  button {
    position: fixed;
    top: 0;
    right: 0;
    z-index: 100;
    text-decoration: underline;
    background-color: transparent;
    border: none;
    }
</style>