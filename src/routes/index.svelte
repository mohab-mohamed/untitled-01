<script lang="ts">
  import Icon from '$lib/atoms/icon/icon.svelte';
  import { default as cloud } from '$lib/assets/icons/cloud';
  import Button from '$lib/atoms/button/button.svelte';
  import DisjointCloud from '$lib/organisms/disjoint-cloud/disjoint-cloud.svelte';
</script>

<Icon data={cloud} />
<Button label={'mohab'} />
<DisjointCloud />
