<script>
     import '../style.css';
     import { writable } from 'svelte/store';
     let todoItem = '';
     let storedList, urgent, someday;
     let todoList = writable([]);
     $: isDone = [];
     $: somedayList = [];
     if (typeof window !== 'undefined' && typeof localStorage !== 'undefined') {
          storedList = localStorage.getItem('storedList');
          if(storedList) {
               $todoList = (JSON.parse(storedList));
          }
     }

     function updateList() {
          return storedList = localStorage.setItem('storedList', JSON.stringify($todoList));
     }


     function addToArray() {
          if (todoItem == '') {
               return;
          }
          $todoList = [...$todoList, {
               text: todoItem,
               done: false,
               urgent: urgent,
               someday: someday
          }];
          isDone = $todoList.filter(item => item.done);
          somedayList = $todoList.filter(item => item.someday);
          console.log(somedayList);
          //console.log($todoList);
          updateList();
          todoItem = '';
          urgent = false; 
          someday = false;
     }
          function removeThis(index) {
          $todoList.splice(index, 1);
          $todoList = $todoList;
          updateList();
     }
     function clearDone() {
          $todoList = $todoList.filter(item => !item.done)
          updateList();
     }
</script>

<h1>Todo!</h1>

<form on:submit|preventDefault={addToArray}>
     <input type="text" bind:value={todoItem}>
     <p>Choose Category:</p>
     <input type="checkbox" name="urgent" id="urgent" bind:checked={urgent}>
     <label for="urgent">Urgent</label>
     <input type="checkbox" name="someday" id="someday" bind:checked={someday}>
     <label for="someday">Some Day</label>
<div>
          <button type="submit">Add</button>
     </div>
</form>

<ul>
     {#each $todoList as item, index}
          <li>
               <input type="checkbox" bind:checked={item.done} on:change={updateList}>

               <span class:done={item.done} >{item.text}</span>
               <span on:click={() => removeThis(index)} class="remove" role="button" tabindex="0">&times;</span>
          </li>
     {/each}
</ul>

{#if somedayList.length > 0}
     <h2>Someday</h2>
     <ul>
          {#each somedayList as item, index}
               <li>{item.text}</li>
          {/each}
     </ul>
{/if}
{#if isDone.length > 0}
<button on:click={clearDone}>Remove Done</button>
{/if}

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
          color: darkred;
          cursor: pointer;
     }
</style>