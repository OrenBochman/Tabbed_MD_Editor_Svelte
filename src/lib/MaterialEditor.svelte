<script lang="ts">
  import Button from '@smui/button';
  import Card, { Content } from '@smui/card';
  import Tab, { Label } from '@smui/tab';
  import TabBar from '@smui/tab-bar';
	import MarkdownIt from 'markdown-it';
	import { afterUpdate } from 'svelte';

  /*todo:

  
  */ 
	let active = "edit";
	const md = new MarkdownIt();
	const tabs=['edit', 'preview']
	let markdown:String = '';
  let output:String = '';
	//$: output:String = md.render(markdown);	
	afterUpdate( () => { output = md.render(markdown) } );
</script>

<svelte:head>
  <!-- Fonts -->
  <link rel="stylesheet" href="https://fonts.googleapis.com/icon?family=Material+Icons" />
  <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Roboto:300,400,500,600,700" />
  <!-- Material Typography -->
  <link rel="stylesheet" href="https://unpkg.com/@material/typography@11.0.0/dist/mdc.typography.css"
  />
  <!-- SMUI -->
  <link rel="stylesheet" href="https://unpkg.com/svelte-material-ui/bare.css" />
</svelte:head>

<section class='editor'>
<div>
  <TabBar {tabs} let:tab bind:active>
    <!-- Note: the `tab` property is required! -->
    <Tab {tab}>      
      <Label>{tab}</Label>
    </Tab>
  </TabBar>
</div>
  {#if active && active === 'edit'}
  <card>
    <textarea bind:value={markdown} ></textarea>  
  </card>
  {:else}
  <card ><div class='output'>
    {@html output}</div>
    </card>
  {/if}

<div style="margin-top: 1em;">
  <div>Programmatically select:</div>
  {#each tabs as tab}
  <Button on:click={() => (active = tab)}><Label>{tab}</Label></Button>
  {/each}
</div>
  <pre class="status">Selected: {active}</pre>
</section>

<style>

.output{
  text-align:left;
}

  textarea {
    box-sizing: inherit;
		height: 40rem;
		width: 100%;
		padding: 1rem;
		border: 1px solid black;
  }
</style>