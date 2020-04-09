<script>
    import { scale } from 'svelte/transition';
    export let id
    export let size = {
        height: Math.round(Math.random() * 200) + 100,
        width: Math.round(Math.random() * 200) + 100,
    }
    export let pos = {
        top: Math.round(Math.random() * (window.innerHeight - 2 * size.height) + 0.5 * size.height),
        left: Math.round(Math.random() * (window.innerWidth - 2 * size.width) + 0.5 * size.width),
        }
    export let effects = {
        shadows: false,
        blur: false,
    }

    let dragging = false;
    let resizing = false;
    let o = { x:0, y:0 }

    function startDrag(e) {
        dragging = true;
        o.y = e.pageY - pos.top
        o.x = e.pageX - pos.left
        document.addEventListener("mousemove", drag)
        document.addEventListener("touchmove", drag)
        document.addEventListener("mouseup", endDrag)
        document.addEventListener("touchend", endDrag)
    }

    function drag(e) {
        pos.top = e.pageY - o.y;
        pos.left = e.pageX - o.x;
        e.preventDefault()
    }

    function endDrag(e) {
        dragging = false;
        document.removeEventListener("mousemove", drag)
        document.removeEventListener("touchmove", drag)
        document.removeEventListener("mouseup", endDrag)
        document.removeEventListener("touchend", endDrag)
    }

    function startResize(e) {
        resizing = true;
        o.y = size.height + pos.top - e.pageY
        o.x = size.width + pos.left - e.pageX
        document.addEventListener("mousemove", resize)
        document.addEventListener("touchmove", resize)
        document.addEventListener("mouseup", endResize)
        document.addEventListener("touchend", endResize)
    }

    function resize(e) {
        size.height = e.pageY - pos.top + o.y;
        size.width = e.pageX - pos.left + o.x;
        e.preventDefault()
    }

    function endResize(e) {
        resizing = false;
        document.removeEventListener("mousemove", resize)
        document.removeEventListener("touchmove", resize)
        document.removeEventListener("mouseup", endResize)
        document.removeEventListener("touchend", endResize)
    }
</script>

<style>
    * {
        user-select: none;
        -webkit-user-select: none;
        cursor: move;
    }

    div {
        display: flex;
        align-items: center;
        border: 1px solid slategray;
        letter-spacing: 1px;

        background-color: white;

        position: absolute;
        border-radius: 10px;
    }

    div.shadow {
        box-shadow: 0 0 30px 1px rgba(0, 0, 0, 0.1);
        border: none;
    }

    div.dragging {
        font-weight: bold;
        letter-spacing: normal;
        background-color: lime;
        color: green;
    }

    div.resizing {
        font-weight: bold;
        letter-spacing: normal;
        background-color: lightcyan;
        outline: 1px dotted lightskyblue;
        outline-offset: 10px;
        color: dodgerblue;
    }

    div.dragging.blur, div.resizing.blur {
        filter: blur(1px)
    }

    p {
        text-align: center;
        margin: auto;
    }
    span {
        position: absolute;
        font-size: 10px;
        font-weight: bold;
        letter-spacing: normal;
        color: dodgerblue;
        text-align: right;
        right: 3px;
    }

    span.exit {
        top: 3px;
        cursor: pointer
    }

    span.size {
        bottom: 3px;
        cursor: nwse-resize
    }
</style>

<div
    on:mousedown={startDrag}
    on:touchstart={startDrag}
    on:mousedown
    on:touchstart
    style='
        top:{pos.top}px;
        left:{pos.left}px;
        height:{size.height}px;
        width:{size.width}px;
    '
    class='
        {dragging && "dragging"}
        {resizing && "resizing"}
        { effects.shadow && "shadow"}
        { effects.blur && "blur"}
    '
    transition:scale={{
        duration: 200
    }}
>
    <p>
        <slot>This is a window.</slot>
    </p>
    <span
        class="exit"
        on:mousedown|stopPropagation
        on:toucstart|stopPropagation
        on:click|stopPropagation
        on:mouseup|stopPropagation
        on:touchend|stopPropagation
    >
        { id }
    </span>
    <span
        class="size"
        on:mousedown|stopPropagation={startResize}
        on:touchstart|stopPropagation={startResize}
    >
        { pos.left }:{ pos.top }
    </span>
</div>