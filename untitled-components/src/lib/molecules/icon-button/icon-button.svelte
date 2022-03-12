<script>
    import Icon from '../../atoms/icon/icon.svelte';

    export let iconData;
    export let iconScale;

    //optional
    export let className = undefined;
</script>

<button class={['p-5 hover:bg-gray-100 rounded-lg', className].join(' ')}>
    <Icon data={iconData} scale={iconScale} />
</button>
