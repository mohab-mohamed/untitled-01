<script>
    import IconButton from '../../molecules/icon-button/icon-button.svelte';
    export let list = [];
</script>

<div class="flex items-center h-18">
    {#each list as item}
        <IconButton className="mx-3" iconData={item.iconData} iconScale={2} />
    {/each}
</div>
