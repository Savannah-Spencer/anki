<!--
Copyright: Ankitects Pty Ltd and contributors
License: GNU AGPL, version 3 or later; http://www.gnu.org/licenses/agpl.html
-->
<script lang="ts">
    import DynamicallySlottable from "../components/DynamicallySlottable.svelte";
    import Item from "../components/Item.svelte";
    import * as tr from "../lib/ftl";
    import CardStateCustomizer from "./CardStateCustomizer.svelte";
    import type { DeckOptionsState } from "./lib";
    import SpinBoxFloatRow from "./SpinBoxFloatRow.svelte";
    import SpinBoxRow from "./SpinBoxRow.svelte";
    import TitledContainer from "./TitledContainer.svelte";

    export let state: DeckOptionsState;
    export let api: Record<string, never>;

    const config = state.currentConfig;
    const defaults = state.defaults;
    const cardStateCustomizer = state.cardStateCustomizer;
</script>

<TitledContainer title={tr.deckConfigAdvancedTitle()}>
    <DynamicallySlottable slotHost={Item} {api}>
        <Item>
            <SpinBoxRow
                bind:value={$config.maximumReviewInterval}
                defaultValue={defaults.maximumReviewInterval}
                min={1}
                max={365 * 100}
                markdownTooltip={tr.deckConfigMaximumIntervalTooltip()}
            >
                {tr.schedulingMaximumInterval()}
            </SpinBoxRow>
        </Item>

        <Item>
            <SpinBoxFloatRow
                bind:value={$config.initialEase}
                defaultValue={defaults.initialEase}
                min={1.31}
                max={5}
                markdownTooltip={tr.deckConfigStartingEaseTooltip()}
            >
                {tr.schedulingStartingEase()}
            </SpinBoxFloatRow>
        </Item>

        <Item>
            <SpinBoxFloatRow
                bind:value={$config.easyMultiplier}
                defaultValue={defaults.easyMultiplier}
                min={1}
                max={3}
                markdownTooltip={tr.deckConfigEasyBonusTooltip()}
            >
                {tr.schedulingEasyBonus()}
            </SpinBoxFloatRow>
        </Item>

        <Item>
            <SpinBoxFloatRow
                bind:value={$config.intervalMultiplier}
                defaultValue={defaults.intervalMultiplier}
                min={0.5}
                max={2}
                markdownTooltip={tr.deckConfigIntervalModifierTooltip()}
            >
                {tr.schedulingIntervalModifier()}
            </SpinBoxFloatRow>
        </Item>

        <Item>
            <SpinBoxFloatRow
                bind:value={$config.hardMultiplier}
                defaultValue={defaults.hardMultiplier}
                min={0.5}
                max={1.3}
                markdownTooltip={tr.deckConfigHardIntervalTooltip()}
            >
                {tr.schedulingHardInterval()}
            </SpinBoxFloatRow>
        </Item>

        <Item>
            <SpinBoxFloatRow
                bind:value={$config.lapseMultiplier}
                defaultValue={defaults.lapseMultiplier}
                max={1}
                markdownTooltip={tr.deckConfigNewIntervalTooltip()}
            >
                {tr.schedulingNewInterval()}
            </SpinBoxFloatRow>
        </Item>

        {#if state.v3Scheduler}
            <Item>
                <CardStateCustomizer bind:value={$cardStateCustomizer} />
            </Item>
        {/if}
    </DynamicallySlottable>
</TitledContainer>
