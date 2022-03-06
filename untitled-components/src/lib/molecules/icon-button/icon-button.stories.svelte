<script>
    import {Meta, Template, Story} from '@storybook/addon-svelte-csf';
    import IconButton from './icon-button.svelte';
    import {default as cloud} from '../../../assets/icons/cloud';
</script>

<!-- More on default export: https://storybook.js.org/docs/react/writing-stories/introduction#default-export -->
<!-- More on argTypes: https://storybook.js.org/docs/svelte/api/argtypes -->
<Meta title="Molecules/Icon Button" component={IconButton} />

<!-- More on component templates: https://storybook.js.org/docs/svelte/writing-stories/introduction#using-args -->
<Template let:args>
    <IconButton {...args} />
</Template>

<!-- More on args: https://storybook.js.org/docs/svelte/writing-stories/args -->
<Story name="Default" args={{}} />

<Story name="Cloud" args={{iconData: cloud}} />
