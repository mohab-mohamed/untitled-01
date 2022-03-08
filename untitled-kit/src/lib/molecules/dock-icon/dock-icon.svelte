<script>
  import Button from '$lib/atoms/button/button.svelte';
  import Icon from '$lib/atoms/icon/icon.svelte';

  export let iconData;
  export let className;

  // optional
  export let iconColor = undefined;

  const handleMouseEnter = () => {
    iconColor = 'primary-shade';
  };

  const handleMouseLeave = () => {
    iconColor = null;
  };
</script>

<Icon
  on:mouseenter={() => handleMouseEnter()}
  on:mouseleave={() => handleMouseLeave()}
  class={['cursor-pointer', className].join(' ')}
  color={iconColor}
  data={iconData}
  scale={3}
/>
