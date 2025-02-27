<script lang="ts">
    import { setContext } from "svelte";
    import Icon from "@iconify/svelte";
    import type { ColorSchema } from "./model/colorSchema";
    import ColorSelector from "./ColorSelector.svelte";
    import { persisted } from "$lib/utils/persisted";

    export var schema: ColorSchema;

    const preferAbgr = setContext("prefer-abgr", persisted("prefer-abgr", false));
</script>

<div class="variant-soft flex w-full flex-col gap-2 rounded-md p-2">
    <div class="variant-ringed flex flex-col rounded-md p-[0.5px]">
        <div class="variant-ringed flex w-full items-center gap-2 rounded-t-md">
            <Icon icon="mdi:content-save-cog" class="h-10 w-10 p-2" />
            <span class="shrink grow"> 方案名稱代碼 </span>
            <input
                type="text"
                bind:value={schema.schemaName}
                class="variant-ringed h-10 w-[40%] rounded-none rounded-tr-md px-2 font-mono hover:variant-ghost"
            />
        </div>
        <div class="variant-ringed flex w-full items-center gap-2">
            <Icon icon="mdi:rename-box" class="h-10 w-10 p-2" />
            <span class="shrink grow"> 方案顯示名稱 </span>
            <input
                type="text"
                bind:value={schema.name}
                class="variant-ringed h-10 w-[40%] rounded-none px-2 hover:variant-ghost"
            />
        </div>
        <div class="variant-ringed flex w-full items-center gap-2 rounded-b-md">
            <Icon icon="mdi:account-box" class="h-10 w-10 p-2" />
            <span class="shrink grow"> 方案作者信息 </span>
            <input
                type="text"
                bind:value={schema.author}
                class="variant-ringed h-10 w-[40%] rounded-none rounded-br-md px-2 hover:variant-ghost"
            />
        </div>
    </div>
    <div class="flex w-full items-center gap-2">
        <Icon icon={$preferAbgr ? "mdi:checkbox-marked" : "mdi:checkbox-blank-off"} class="h-10 w-10 p-2" />
        <button
            class="shrink grow"
            on:click={() => {
                $preferAbgr = !$preferAbgr;
            }}
        >
            使用 ABGR 顔色格式
        </button>
        <span class="code w-6"> {$preferAbgr ? "是" : "否"} </span>
    </div>
    <div class="flex w-full items-center gap-2">
        <Icon icon="mdi:rounded-corner" class="h-10 w-10 p-2" />
        <span class="shrink grow"> 按鍵圓角 </span>
        <input type="range" min="0" max="32" bind:value={schema.corner_radius} class="w-[30%]" />
        <span class="code w-6"> {schema.corner_radius} </span>
    </div>
    <ColorSelector title="鍵盤背景" bind:color={schema.back_color} />
    <ColorSelector title="按鍵背景" bind:color={schema.button_back_color} />
    <ColorSelector title="按鍵按下背景" bind:color={schema.button_pressed_back_color} />
    <ColorSelector title="按鍵氣泡背景" bind:color={schema.button_bubble_back_color} />
    <ColorSelector title="按鍵文字" bind:color={schema.button_front_color} />
    <ColorSelector title="按鍵按下文字" bind:color={schema.button_pressed_front_color} />
    <ColorSelector title="按鍵劃動文字" bind:color={schema.button_swipe_front_color} />
    <ColorSelector title="按鍵按下劃動文字" bind:color={schema.button_swipe_pressed_front_color} />
    <div class="flex w-full items-center gap-2">
        <Icon icon="mdi:format-size" class="h-10 w-10 p-2" />
        <span class="shrink grow"> 鍵面字體大小 </span>
        <input type="range" min="0" max="32" bind:value={schema.font_size} class="w-[30%]" />
        <span class="code w-6"> {schema.font_size} </span>
    </div>
    <div class="flex w-full items-center gap-2">
        <Icon icon="mdi:format-size" class="h-10 w-10 p-2" />
        <span class="shrink grow"> 劃動字體大小 </span>
        <input type="range" min="0" max="24" bind:value={schema.swipe_font_size} class="w-[30%]" />
        <span class="code w-6"> {schema.swipe_font_size} </span>
    </div>
    <ColorSelector title="按鍵邊框" bind:color={schema.border_color} />
    <div class="flex w-full items-center gap-2">
        <Icon icon="mdi:border-all-variant" class="h-8 w-10 px-1" />
        <span class="shrink grow"> 邊框寛度 </span>
        <input type="range" min="0" max="8" bind:value={schema.border_size} class="w-[30%]" />
        <span class="code w-6"> {schema.border_size} </span>
    </div>
    <ColorSelector title="按鍵下邊框" bind:color={schema.lower_edge_color} />
    <ColorSelector title="按鍵陰影" bind:color={schema.shadow_color} />
    <div class="flex w-full items-center gap-2">
        <Icon icon="mdi:format-size" class="h-10 w-10 p-2" />
        <span class="shrink grow"> 按鍵陰影大小 </span>
        <input type="range" min="0" max="8" bind:value={schema.shadow_size} class="w-[30%]" />
        <span class="code w-6"> {schema.shadow_size} </span>
    </div>
    <ColorSelector title="預編輯區文字" bind:color={schema.text_color} />
    <ColorSelector title="首選文字" bind:color={schema.hilited_candidate_text_color} />
    <ColorSelector title="首選註釋" bind:color={schema.hilited_comment_text_color} />
    <ColorSelector title="首選背景" bind:color={schema.hilited_candidate_back_color} />
    <ColorSelector title="首選序號" bind:color={schema.hilited_candidate_label_color} />
    <ColorSelector title="次選文字" bind:color={schema.candidate_text_color} />
    <ColorSelector title="次選註釋" bind:color={schema.comment_text_color} />
    <ColorSelector title="次選序號" bind:color={schema.label_color} />
    <ColorSelector title="長按首選背景" bind:color={schema.hilited_callout_back_color} />
    <ColorSelector title="長按首選文字" bind:color={schema.hilited_callout_foreground_color} />
    <ColorSelector title="長按氣泡背景" bind:color={schema.action_callout_back_color} />
</div>
