<script>
import TodoForm from "./lib/Todo/TodoForm.svelte";
import TodoList from "./lib/Todo/TodoList.svelte";

let storageItems = JSON.parse(localStorage.getItem('items'))
let items = []

if(storageItems) {
  items = storageItems
}

function addItemHandle(event) {
  add(event.detail)
}
function add(item){
  items = [item, ...items]
  localStorage.removeItem('items')
  localStorage.setItem('items', JSON.stringify(items))
}

function updateItemHandle(event){
  remove(event.detail.editedId)
  add({
    name: event.detail.updatedName,
    desc: event.detail.updatedDesc,
    id: event.detail.editedId
  })
}

function removeItemHandle(event){
  let itemId = event.detail.id
  remove(itemId)
}
function remove(itemId){
  let newItem = items.filter(item => {
    return item.id != itemId
  })
  items = [...newItem]
  localStorage.removeItem('items')
  localStorage.setItem('items', JSON.stringify(items))
}

let editItem = null
function editItemHandle(event){
  let item = event.detail.item
  editItem = item
}

</script>

<main>
  <h1>The Minimal To Do</h1>
  <TodoForm 
    on:addItem={addItemHandle} 
    on:updateItem={updateItemHandle} 
    {editItem}
  />
  <TodoList 
    bind:items 
    on:removeItem={removeItemHandle}
    on:editItem={editItemHandle}  
  />

  <footer>
    <p>Vite + Svelte + Nes.css</p>
  </footer>
</main>

<style>
  h1 {
    font-size: 3.2em;
    line-height: 1.1;
  }
  footer {
    height: 2rem;
    padding-top: 0.6rem;
    position: fixed;
    left: 0;
    bottom: 0;
    width: 100%;
    background-color: rgb(235, 235, 235);
    color: rgb(179, 179, 179);
    text-align: center;
  }

  footer p {
    font-size: 0.6rem !important;
  }

</style>