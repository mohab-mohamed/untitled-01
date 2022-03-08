<script lang="ts">
  import Icon from '$lib/atoms/icon/icon.svelte';

  import { default as pie } from '../../assets/icons/pie-chart';
  import { default as calendar } from '../../assets/icons/calendar';
  import { default as cloud } from '../../assets/icons/cloud';
  import { default as list } from '../../assets/icons/list';
  import { default as gear } from '../../assets/icons/gear';

  import Button from '$lib/atoms/button/button.svelte';
  import DisjointCloud from '$lib/organisms/disjoint-cloud/disjoint-cloud.svelte';
  import DockIcon from '$lib/molecules/dock-icon/dock-icon.svelte';
  import Dock from '$lib/organisms/dock/dock.svelte';
  import IconButton from '$lib/molecules/icon-button/icon-button.svelte';
  import SearchField from '$lib/molecules/search-field/search-field.svelte';

  const dockList = [
    { iconData: pie },
    { iconData: calendar },
    { iconData: cloud },
    { iconData: list },
    { iconData: gear },
  ];
</script>

<Icon data={cloud} />
<br />
<br />
<Button label={'mohab'} />
<br />
<DisjointCloud />
<br />
<DockIcon className={null} iconData={cloud} />
<br />
<br />
<Dock list={dockList} />
<br />
<IconButton iconData={cloud} />
<br />
<div class="w-96">
  <SearchField />
</div>
