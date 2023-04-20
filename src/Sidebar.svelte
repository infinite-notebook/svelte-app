<script lang="ts">
  import * as Y from 'yjs'

  const truncate = (str: string, n: number) => {
    return str.length > n ? str.substr(0, n - 1) + '...' : str
  }

  export let yXmlFragment: Y.XmlFragment

  let list = yXmlFragment.toArray()

  yXmlFragment.observeDeep(() => {
    list = yXmlFragment.toArray()
  })
</script>

{#each list as node}
  {#if node instanceof Y.XmlElement && node.firstChild !== null}
    <div style={node.nodeName == 'heading' ? 'font-weight: bold;' : ''}>
      {truncate(node.firstChild.toString(), 15)}
    </div>
  {/if}
{/each}

<style>
  div {
    padding: 10px 10px;
  }
</style>