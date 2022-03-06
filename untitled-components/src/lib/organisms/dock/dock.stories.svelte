<script>
    import {Meta, Template, Story} from '@storybook/addon-svelte-csf';
    import Dock from './dock.svelte';
    import {default as pie} from '../../../assets/icons/pie-chart';
    import {default as calendar} from '../../../assets/icons/calendar';
    import {default as cloud} from '../../../assets/icons/cloud';
    import {default as list} from '../../../assets/icons/list';
    import {default as gear} from '../../../assets/icons/gear';

    const dockList = [{iconData: pie}, {iconData: calendar}, {iconData: cloud}, {iconData: list}, {iconData: gear}];
</script>

<!-- More on default export: https://storybook.js.org/docs/react/writing-stories/introduction#default-export -->
<!-- More on argTypes: https://storybook.js.org/docs/svelte/api/argtypes -->
<Meta title="Organisms/Dock" component={Dock} />

<!-- More on component templates: https://storybook.js.org/docs/svelte/writing-stories/introduction#using-args -->
<Template let:args>
    <Dock {...args} />
</Template>

<!-- More on args: https://storybook.js.org/docs/svelte/writing-stories/args -->
<Story name="Default" args={{}} />
<Story name="Mock Dock List" args={{list: dockList}} />
