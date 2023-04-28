<script>
    export let items;
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    function removeHandle(id) {
        dispatch('removeItem', {
            id
        })
    }

    function editHandle(item) {
        dispatch('editItem', {
            item
        })
    }

</script>

<section>
    {#each items as item}
        <div class="nes-container with-title is-centered margin">
            <p class="title inner-title">{item.name}</p>
            <p>{item.desc}</p>
            <div>
                <button type="button" class="nes-btn is-warning" on:click={() => editHandle(item)}>Edit</button>
                <button type="button" class="nes-btn is-error" on:click={() => removeHandle(item.id)}>Remove</button>
            </div>
        </div>
    {/each}
</section>

<style>
.margin {
    margin: 2rem;
}
.inner-title {
    font-size: 1.5rem !important;
}
</style>