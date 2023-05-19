<script lang="ts">
    export let color: string = "#FFF";
    export let icon: string = null;

    let xPos = 10;
    let yPos = 10;

    let element;

    function moveBg(e) {
        let rect = element.getBoundingClientRect();

        xPos = (e.clientX - rect.left)*0.9;
        yPos = (e.clientY - rect.top)*0.9;
    }
</script>

<div class="crumb" on:mousemove={moveBg} style="--xPos: {xPos}px; --yPos: {yPos}px; --color: {color}" bind:this={element}>
    {#if icon != null}
        <img src={icon} alt="icon">
    {/if}
    <slot />
</div>

<style lang="scss">
    .crumb {
        display: flex;
        background-color: rgba($color: #FFF, $alpha: 0.1);
        padding: .75em;
        border: rgba($color: #FFF, $alpha: 0.3) 1px solid;
        border-radius: 8px;
        white-space: nowrap;
        align-items: center;

        --xPos: 10px;
        --yPos: 10px;

        transition: background 250ms, scale 250ms;

        &:hover {
            background: radial-gradient(
                35rem circle at var(--xPos) var(--yPos),
                var(--color),
                rgba($color: #FFF, $alpha: 0.1) 15%
            );
            scale: 1.1;
        }

        img {
            max-height: 1em;
            margin-right: 5px;
        }
    }
</style>
