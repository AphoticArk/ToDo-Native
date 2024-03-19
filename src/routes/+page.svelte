<script>
     import '../style.css';
     let todoItem = '';
     let todoList = [];

     function addToArray() {
          if (todoItem == '') {
               return;
          }
          todoList = [...todoList,{
               text: todoItem,
               done: false
          }];
          console.log(todoList);
          todoItem = '';
     }
     function removeThis(index) {
          todoList.splice(index, 1)
          todoList = todoList;
     }
</script>

<h1>Todo.</h1>

<form on:submit|preventDefault={addToArray}>
     <input type="text" bind:value={todoItem}>
     <button type="submit">Add</button>
</form>

<ul>
     {#each todoList as item, index}
          <li>
               <input type="checkbox" bind:checked={item.done}>
               <span class:done={item.done}>{item.text}</span>
               <span on:click={() => removeThis(index)} class="remove" role='button' tabindex='0'>&times;</span>
          </li>
     {/each}
</ul>

<style> 
     ul {
          list-style: none;
     }
     li {
          font-size: 1.3rem;
     }
     .done {
          color: #ccc;
          text-decoration: line-through;
     }
     .remove {
          color: #c00;
          cursor: pointer;
     }

</style>