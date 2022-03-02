<script>
  export let label;
</script>

<button
  class={[
    'px-24',
    'py-2',
    'bg-gray-100',
    'rounded',
    'border',
    'border-gray-200',
    'hover:bg-gray-800',
    'hover:text-gray-200',
  ].join(' ')}
>
  {label}
</button>

<style>
</style>
