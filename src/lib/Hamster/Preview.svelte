<script lang="ts">
    import { createEventDispatcher, getContext } from "svelte";
    import type { Writable } from "svelte/store";
    import Icon from "@iconify/svelte";
    import type { ColorSchema, KeyStyle } from "./model/colorSchema";
    import type { Keyboard } from "./model/keyboardLayout";
    import KeyPreview from "./KeyPreview.svelte";

    export var sticky: boolean = true;
    export var landscape: boolean = false;
    export var schema: ColorSchema;
    export var keyboard: Keyboard;
    export var keyStyleMap: { [name: string]: KeyStyle } = {};

    export var selected: { row: number; col: number };

    var darkMode: Writable<boolean> = getContext("darkMode");
    var previewWidth: number = 0;

    const dispatch = createEventDispatcher();
    const onClick = (row: number, col: number) => {
        dispatch("clicked", { row, col });
    };
    const onStickyClick = () => {
        dispatch("sticky");
    };
</script>

<div
    bind:clientWidth={previewWidth}
    style:background-color={schema.back_color.rgba()}
    class="w-full rounded-md backdrop-blur"
>
    <!-- preedit -->
    <div class="w-full px-2">
        <span style:color={schema.text_color.rgba()} class="px-1 text-xs"> lleiu </span>
    </div>
    <!-- candidates -->
    <div class="flex w-full items-center gap-1 px-2 text-sm">
        {#each ["卢老师", "罗老师", "罗利市"] as cand, index}
            <span
                style:color={index === 0
                    ? schema.hilited_candidate_text_color.rgba()
                    : schema.candidate_text_color.rgba()}
                style:background-color={index === 0 ? schema.hilited_candidate_back_color.rgba() : undefined}
                class="text-md text-nowrap rounded-md px-1"
            >
                <span
                    style:color={index == 0 ? schema.hilited_candidate_label_color.rgba() : schema.label_color.rgba()}
                >
                    {index + 1}.
                </span>
                {cand}
                <span
                    style:color={index === 0
                        ? schema.hilited_comment_text_color.rgba()
                        : schema.comment_text_color.rgba()}
                    class="text-xs"
                >
                    {["lleiu", "lleiui", "lleiuo"][index]}
                </span>
            </span>
        {/each}
        <span class="grow" />
        <button
            title={(landscape ? "預覽: 横屏" : "預覽: 竪屏") + "\n注: 行高和鍵寛的配置值在横竪屏模式下是各自獨立的"}
            on:click={() => {
                landscape = !landscape;
            }}
            style:color={schema.comment_text_color.rgba()}
        >
            <Icon height="20" icon={landscape ? "mdi:crop-landscape" : "mdi:crop-portrait"} />
        </button>
        <button
            title={sticky ? "取消置頂" : "置頂預覽"}
            on:click={onStickyClick}
            style:color={schema.comment_text_color.rgba()}
        >
            <Icon height="20" icon={sticky ? "mdi:pin" : "mdi:pin-off"} />
        </button>
    </div>
    <!-- keyboard layout -->
    {#each keyboard.rows as row, indexRow (row.id)}
        <div style:height={`${landscape ? row.landscapeHeight || 56 : row.rowHeight || 56}px`} class="flex w-full">
            {#each row.keys as key, indexKey (key.id)}
                <KeyPreview
                    {landscape}
                    {schema}
                    {row}
                    {key}
                    keyStyle={keyStyleMap[$darkMode ? key.darkStyle : key.lightStyle]}
                    insets={keyboard.buttonInsets}
                    selected={selected.row === indexRow && selected.col === indexKey}
                    coordinate={selected}
                    {previewWidth}
                    on:clicked={() => {
                        onClick(indexRow, indexKey);
                    }}
                />
            {/each}
        </div>
    {/each}
</div>
