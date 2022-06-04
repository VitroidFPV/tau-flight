<div class="flex h-fit">
    <input type="number" value={value} id={id} min={min} max={max} step={step} class="w-16 bg-transparent border-b-2 border-zinc-600 focus:border-yellow-400 hover:border-yellow-300 focus:outline-none duration-300">
    <label for={id} class="ml-2">{text}</label>
</div>

<script lang="ts"> 
    export let id: string
    export let min: number
    export let max: number
    export let step: number
    export let text: string
    export let value: number
</script>