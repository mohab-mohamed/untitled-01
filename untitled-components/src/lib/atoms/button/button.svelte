<script>
    export let label = 'default label';
</script>

<button
    class={[
        'px-24',
        'py-2',
        'bg-gray-100',
        'rounded',
        'border',
        'border-gray-200',
        'hover:bg-gray-500',
        'hover:text-gray-200'
    ].join(' ')}
>
    {label}
</button>

<style>
</style>
