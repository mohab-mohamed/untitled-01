<script>
  import Button from '$lib/atoms/button/button.svelte';
  import { default as pie } from '../../assets/icons/pie-chart';
  import { default as calendar } from '../../assets/icons/calendar';
  import { default as cloud } from '../../assets/icons/cloud';
  import { default as list } from '../../assets/icons/list';
  import { default as gear } from '../../assets/icons/gear';
  import Dock from '$lib/organisms/dock/dock.svelte';
  import DisjointCloud from '$lib/organisms/disjoint-cloud/disjoint-cloud.svelte';
  import SearchField from '$lib/molecules/search-field/search-field.svelte';

  const dockList = [
    { iconData: pie },
    { iconData: calendar },
    { iconData: cloud },
    { iconData: list },
    { iconData: gear },
  ];
</script>

<div class={['w-screen', 'h-screen', 'flex', 'flex-col'].join(' ')}>
  <div
    class={['w-full', 'h-1/10', 'flex', 'justify-between', 'items-center'].join(
      ' '
    )}
  >
    <div><Button label={'mohab'} /></div>
    <div class={['w-96'].join(' ')}><SearchField /></div>
    <div><Button label={'mohab'} /></div>
  </div>
  <div
    class={['w-full', 'h-8/10', 'flex', 'justify-center', 'items-center'].join(
      ' '
    )}
  >
    <div><DisjointCloud /></div>
  </div>
  <div
    class={['w-full', 'h-1/10', 'flex', 'justify-between', 'items-center'].join(
      ' '
    )}
  >
    <div>parent</div>
    <div><Dock list={dockList} /></div>
    <div>child</div>
  </div>
</div>
