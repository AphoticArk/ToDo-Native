<script>
     // look at svelte transition library 
	import { slide } from 'svelte/transition';
	import { quintOut } from 'svelte/easing';
     import '../style.css';
     import {writable} from 'svelte/store';
     let todoItem = '';
     let storedListNotDone;
     let storedListDone;
     let todoList = writable([]);
     let finishedList = writable([]);
     let temp = '';

     if (typeof window !== 'undefined' && typeof localStorage !== 'undefined') {
          storedListNotDone = localStorage.getItem('storedListNotDone');
          if (storedListNotDone) {
               $todoList = (JSON.parse(storedListNotDone));
          }
     }

     if (typeof window !== 'undefined' && typeof localStorage !== 'undefined') {
          storedListDone = localStorage.getItem('storedListDone');
          if (storedListDone) {
               $finishedList = (JSON.parse(storedListDone));
          }
     }

     function updateTaskList() {
          return storedListNotDone = localStorage.setItem('storedListNotDone', JSON.stringify($todoList));
     }
     function updateDoneList() {
          return storedListDone = localStorage.setItem('storedListDone', JSON.stringify($finishedList));
     }

     $: isDone = $finishedList.filter(item => item.done);

     function addToArray() {
          if (todoItem == '') {
               return;
          }
          $todoList = [...$todoList,{
               text: todoItem,
               done: false
          }];
          // console.log(todoList);
          updateTaskList();
          updateDoneList();
          todoItem = '';
     }
     function removeThisTask(index) {

          $todoList.splice(index, 1);
          $todoList = $todoList;
          updateTaskList();
          updateDoneList();
          
     }
     function removeThisDone(index) {

          $finishedList.splice(index, 1);
          $finishedList = $finishedList;
          updateTaskList();
          updateDoneList();
          
     }

     function pushThis(item, index) {

          temp = item;
          $todoList.splice(index, 1);
          $todoList = $todoList;
          $finishedList.push(temp);
          $finishedList = $finishedList;
          updateTaskList();
          updateDoneList();
          console.log(temp);
     }
     // function pushBack(item, index) {

     //      temp = item;
     //      $finishedList.splice(index, 1);
     //      $finishedList = $finishedList;
     //      $todoList.push(temp);
     //      $todoList = $todoList;
     //      updateTaskList();
     //      updateDoneList();
     //      console.log(temp);
     // }
     function clearDone() {
          $finishedList = $finishedList.filter(item => !item.done)
          updateList();
     }
</script>

<div class="frame"> 
     <h1>Todo.</h1>

     <form on:submit|preventDefault={addToArray}>
          <input type="text" bind:value={todoItem}>
          <button type="submit">Add</button>
     </form>

     <ul class="notDone">
          {#each $todoList as item, index}
               <img class="divide" src="https://riyogames.com/img/ui/rule_of_law.png" alt="">
               <li transition:slide={{ duration: 300, easing: quintOut, axis: 'x' }}>
                    <input type="checkbox" bind:checked={item.done} on:click={() => setTimeout(() => pushThis(item, index), 0)} >

                    <span class:done={item.done}>{item.text}</span>
                    <span on:click={() => removeThisTask(index)} class="remove" role='button' tabindex='0'>&times;</span>
               </li>
          {/each}
     </ul>

     {#if isDone.length > 0}
          <h2>Completed</h2>
     {/if}

     <ul class="finishedList">
          {#each $finishedList as item, index}
               <img class="divide" src="https://riyogames.com/img/ui/rule_of_law.png" alt="">
               <li transition:slide={{ duration: 300, easing: quintOut, axis: 'x' }}>
                    
                    <span class="check">&check;</span>
                    <span class:done={item.done}>{item.text}</span>
                    <span on:click={() => removeThisDone(index)} class="remove" role='button' tabindex='0'>&times;</span>
               </li>
          {/each}
     </ul>

     {#if isDone.length > 0}
          <button class='clear' on:click={clearDone}>Remove Done</button>
     {/if}
</div>
<!-- {#if isDone.length > 0} -->
<style> 

     .frame {
          max-width: 600px;
          margin: 4vw auto;
     }
     h1, h2, form, li, span{
          display: flex;
          justify-content: center;
          color: darkslateblue;
     }
     ul img {
          display: flex;
          justify-content: center !important;
          
     }
     h1 {
          font-weight: 700;
          color: midnightblue;
          font-size: 5em;
     }
     h2 {
          margin: 5vw 2vw 1.2vw 0;
          color: midnightblue;
          font-size: 1.8em;

     }
     form {
          margin-top: 2vw;
          margin-bottom: 4vw;
          font-weight: 400;
     }
     input {
          margin-left: 1.5em;
          margin-right: .7em;
          background-color: skyblue;
          color: darkslateblue;
     }
     form input[type="text"] {
          padding: .4em .8em;
          font-size: 1.2rem;
          font-family: "Volkhov", serif;
          border-radius: 7px;
     }
     li input[type="checkbox"] {
          width: 1.75em;
          height: 1.75em;
     }
     button {
          padding: 0em 1.1em;
          font-size: 1.2rem;
          background-color: royalblue;
          color:darkturquoise;
          border-radius: 6px;
     }
     ul {
          list-style: none;
          margin-right: 5vw;
     }
     li {
          font-size: 1.5rem;
          /* margin-right: 10vw; */
          font-weight: 400;
     }
     span {
          margin: 0 .3vw;

     }
     .divide {
          height: 100%;
          width: 30%;
          margin: .5em auto;
     }
     .remove {
          color: #c00;
          cursor: pointer;
          font-size: 1.2em;
          margin-left: 2vw;
     }
     .notDone li {
          color: #000;
          text-decoration: none;
     }
     .finishedList .done{
          color: lightslategray;
          text-decoration: line-through !important;
     }
     .check {
          background-color: blue;
          color: whitesmoke;
          border-radius: 5px;
          height: min-content;
          padding: 0 .2em;
     }
     .clear {
          display: flex;
          justify-content: center !important;
          margin: auto;
          margin-top: 2vw;
          padding: .3em .9em;
          font-size: 1.2rem;
          background-color: royalblue;
          color:darkturquoise;
          border-radius: 6px;
     }

</style>