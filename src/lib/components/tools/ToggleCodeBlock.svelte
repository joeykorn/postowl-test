<script>
  import { classNames } from '$lib/util';
  import { setBlockType } from 'prosemirror-commands';
  import { blockTypeActive } from '../../prosemirrorUtil';

  export let editorView;
  export let editorState;

  $: schema = editorState.schema;
  $: disabled =
    !setBlockType(schema.nodes.code_block)(editorState) &&
    !setBlockType(schema.nodes.paragraph)(editorState);
  $: active = blockTypeActive(schema.nodes.code_block)(editorState);

  function handleClick() {
    if (active) {
      setBlockType(schema.nodes.paragraph)(editorState, editorView.dispatch);
    } else {
      setBlockType(schema.nodes.code_block)(editorState, editorView.dispatch);
    }
    editorView.focus();
  }
</script>

<button
  on:click={handleClick}
  {disabled}
  class={classNames(
    active ? 'bg-white text-black' : 'text-white hover:bg-white hover:text-black',
    'sm:mx-1 rounded-full p-2 disabled:opacity-30'
  )}
>
  <slot />
</button>
