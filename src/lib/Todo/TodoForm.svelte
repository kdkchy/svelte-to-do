<script>
    export let editItem;

    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();

    $: name = editItem?.desc || ''
    $: desc = editItem?.desc || ''

    let isValid = true
    let message = ''
    function addItem(event) {
        event.preventDefault()
        isValid = true
        message = ''
        validation()

        if(isValid) {
            dispatch('addItem', {
                name,
                desc,
                id: Date.now()
            });
            message = 'Saved!'
            resetForm()
        } else {
            message = 'Bad value given!'
        }
    }

    function resetForm(){
        name = ''
        desc = ''
    }

    function validation() {
        if(
            name.length < 1 ||
            desc.length < 1
        ) {
            isValid = false
            return
        }
    }

    function updateItem(event){
        event.preventDefault()
        isValid = true
        message = ''
        validation()

        if(isValid) {
            dispatch('updateItem', {
                updatedName: editItem.name,
                updatedDesc: editItem.desc,
                editedId: editItem.id
            });
            message = 'Updated!'
            editItem = null
            resetForm()
        } else {
            message = 'Bad value given!'
        }
    }
</script>

<section>
    <div>
        <div on:mouseenter={() => {message= ''}} class="message">
            <span class="nes-text {isValid ? "is-success" : "is-error"}">{message}</span>
        </div>
        <form on:submit={addItem}>
            <div class="nes-field wrapper">
                <label for="name">Name</label>
                {#if editItem?.name}
                    <input 
                        type="text" 
                        id="name" 
                        class="nes-input"
                        bind:value={editItem['name']}
                    />
                {:else}
                    <input 
                        type="text" 
                        id="name" 
                        class="nes-input"
                        bind:value={name}
                    />
                {/if}
            </div>
            <div class="nes-field wrapper">
                <label for="textarea_field">Description</label>
                {#if editItem?.desc}
                    <textarea 
                        id="textarea_field" 
                        class="nes-textarea"
                        bind:value={editItem['desc']}    
                    ></textarea>
                {:else}
                    <textarea 
                        id="textarea_field" 
                        class="nes-textarea"
                        bind:value={desc}    
                    ></textarea>
                {/if}
                
            </div>
            <div class="wrapper">
                {#if editItem}
                    <button type="button" class="nes-btn" on:click={() => {editItem = null; resetForm()}}>Cancel</button>  
                    <button type="button" class="nes-btn is-warning" on:click={updateItem}>Edit</button>                
                {:else}
                    <button type="submit" class="nes-btn is-primary">+ Add</button>
                {/if}
            </div>
        </form>
    </div>
</section>

<style>
.wrapper {
    margin: 1rem;
}

.message {
    text-align: center;
}

form {
    margin-bottom: 5rem;
}
</style>