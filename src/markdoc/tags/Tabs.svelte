<script lang="ts" context="module">
    import type { Writable } from 'svelte/store';
    export type TabsContext = Writable<{
        content: ReturnType<typeof createTabs>['elements']['content'];
        triggers: Map<string, string>;
    }>;
</script>

<script lang="ts">
    import { createTabs } from '@melt-ui/svelte';
    import { setContext } from 'svelte';
    import { writable } from 'svelte/store';

    const {
        elements: { root, list, content, trigger },
        states: { value }
    } = createTabs();

    const ctx = setContext<TabsContext>(
        'tabs',
        writable({
            content,
            triggers: new Map()
        })
    );
</script>

<div class="web-card is-normal mt-4" {...$root} use:root>
    <div
        class="tabs flex gap-4 overflow-scroll"
        style="scrollbar-width: none; -ms-overflow-style: none;"
    >
        <ul class="tabs-list flex items-center gap-4" {...$list} use:list>
            {#each $ctx.triggers.entries() as [id, title]}
                <li class="tabs-item rounded-t-[0.625rem] hover:bg-white/[0.04]">
                    <button
                        class="tabs-button cursor-pointer bg-clip-padding py-[0.625rem] px-1 font-light outline-none"
                        class:is-selected={$value === id}
                        {...$trigger(id)}
                        use:trigger>{title}</button
                    >
                </li>
            {/each}
        </ul>
    </div>
    <slot />
</div>
