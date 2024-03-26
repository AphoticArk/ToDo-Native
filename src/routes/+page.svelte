<script>
     import '../style.css';
     import {writable} from 'svelte/store';
     let todoItem = '';
     let storedList;
     let todoList = writable([]);

     if (typeof window !== 'undefined' && typeof localStorage !== 'undefined') {
          storedList = localStorage.getItem('storedList');
          if (storedList) {
               $todoList = (JSON.parse(storedList));
          }
     }

     function updateList() {
          return storedList = localStorage.setItem('storedList', JSON.stringify($todoList));
     }

     $: isDone = $todoList.filter(item => item.done);

     function addToArray() {
          if (todoItem == '') {
               return;
          }
          $todoList = [...$todoList,{
               text: todoItem,
               done: false
          }];
          // console.log(todoList);
          updateList();
          todoItem = '';
     }
     function removeThis(index) {
          $todoList.splice(index, 1)
          $todoList = $todoList;
          updateList();
     }
</script>

<h1>Todo.</h1>

<form on:submit|preventDefault={addToArray}>
     <input type="text" bind:value={todoItem}>
     <button type="submit">Add</button>
</form>

<ul>
     {#each $todoList as item, index}
          <li>
               <input type="checkbox" bind:checked={item.done} on:click={() => setTimeout(() => removeThis(index), 3000)} >

               <span class:done={item.done}>{item.text}</span>
               <span on:click={() => removeThis(index)} class="remove" role='button' tabindex='0'>&times;</span>
          </li>
     {/each}
</ul>
<!-- {#if isDone.length > 0} -->
<style> 
     h1, form, li{
          display: flex;
          justify-content: center;
     }

     form {
          margin-top: 3vw;
          margin-bottom: 4vw;
     }
     ul {
          list-style: none;
     }
     li {
          font-size: 1.3rem;
          margin-right: 3vw;
     }
     span {
          margin: 0 .3vw;
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