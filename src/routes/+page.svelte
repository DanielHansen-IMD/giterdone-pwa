<script>
     import '../style.css';
     let todoItem = '';
     let urgent, someday;
     let todoList = [];
     $: isDone = todoList.filter(item => item.done);
     $: somedayList = todoList.filter(item => item.someday)
     function addToArray() {
          if (todoItem == '') {
               return;
          }
          todoList = [...todoList, {
               text: todoItem,
               done: false,
               urgent: urgent,
               someday: someday
          }];
          console.log(todoList);
          todoItem = '';
     }
     function removeThis(index) {
          todoList.splice(index, 1);
          todoList = todoList;
     }
     function clearDone() {
          todoList = todoList.filter(item => !item.done)
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
     {#each todoList as item, index}
          <li>
               <input type="checkbox" bind:checked={item.done}>

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