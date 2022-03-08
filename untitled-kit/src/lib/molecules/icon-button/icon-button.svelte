<script>
  import Icon from '$lib/atoms/icon/icon.svelte';

  export let iconData;

  //optional
  export let className = undefined;
</script>

<button class={['p-5 hover:bg-gray-100 rounded-lg', className].join(' ')}>
  <Icon data={iconData} scale={3} />
</button>
