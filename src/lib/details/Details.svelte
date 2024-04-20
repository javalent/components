<script lang="ts">
    import { setIcon } from "obsidian";

    export let collapseIcon = "chevron-right";
    export let open = true;
    export let name: string;
    export let desc: string = "";
    export let alwaysOpen = false;

    const details = (node: HTMLDetailsElement) => {
        if (open) node.setAttribute("open", "open");
    };
    const handle = (node: HTMLElement) => {
        setIcon(node, collapseIcon);
    };
</script>

<details
    class="nested-settings setting-item"
    class:always-open={alwaysOpen}
    bind:open
    use:details
>
    <summary
        class="nested-summary"
        on:keyup={(evt) => evt.preventDefault()}
    >
        <div class="setting-item setting-item-heading">
            <div class="setting-item-info">
                <div class="setting-item-name">{name}</div>
                <div class="setting-item-description">{desc}</div>
            </div>
        </div>
        <div class="right-side">
            {#if open}
                <slot name="context" class="context" />
            {/if}
            <div class="collapser">
                <div class="warning-container">
                    <div class="handle" use:handle />
                </div>
            </div>
        </div>
    </summary>

    <div class="settings-container">
        <slot />
    </div>
</details>

<style>
    .always-open {
        pointer-events: none;
    }
    .settings-container {
        pointer-events: initial;
    }
    .nested-settings {
        position: relative;
    }
    .nested-summary {
        outline: none;
        list-style: none !important;
        list-style-type: none !important;
        min-height: 1rem;
        border-top-left-radius: 0.1rem;
        border-top-right-radius: 0.1rem;
        cursor: pointer;
        background-color: var(--background-color);
        margin-right: 0;
        display: flex;
        justify-content: space-between;
    }
    .right-side {
        display: flex;
        align-items: center;
        gap: 1rem;
    }
    summary::-webkit-details-marker,
    summary::marker {
        display: none !important;
    }
    .always-open .handle {
        display: none;
    }
    .collapser {
        display: flex;
        flex-flow: column;
        justify-content: flex-start;
        align-items: flex-end;
        content: "";
    }

    .handle {
        transform: rotate(0deg);
        transition: transform 0.25s;
        display: flex;
    }

    details[open] .handle {
        transform: rotate(90deg);
    }

    .settings-container {
        padding: 0.75em var(--size-4-3);
    }

    .nested-settings {
        border-top: 0px;
    }
</style>
