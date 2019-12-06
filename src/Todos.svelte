<script>
    import TodoItem from "./TodoItem.svelte";

    let newTodoTitle = "";
    let currentFilter = "all";
    let nextId = 4;

    let todos = [
        {
            id: 1,
            title: "My first todo",
            completed: false
        },

         {
            id: 2,
            title: "My second todo",
            completed: false
        },

         {
            id: 3,
            title: "My third todo",
            completed: false
        }
    ];

    // adding new Todo
    function addTodo(event) {
    if (event.key === "Enter") {
        todos = [...todos, {
            id: nextId,
            completed: false,
            title: newTodoTitle
        }];

        // increase nextId
        nextId = nextId + 1;
        newTodoTitle = "";

    }
}

// computed properties
$: todosRemaining = filteredTodos.filter(todo => !todo.completed).length;
$: filteredTodos = currentFilter === "all" ? todos : currentFilter === "completed" ? todos.filter(todo => todo.completed) : todos.filter(todo => !todo.completed)

function checkAllTodos(event) {
    todos.forEach(todo => todo.completed = event.target.checked);
    todos = todos;

}

function updateFilter(newFilter) {
    currentFilter = newFilter;
}

function clearCompleted() {
    todos = todos.filter(todo => !todo.completed);
}

function handleDeleteTodo(event) {
    todos = todos.filter(todo => todo.id !== event.detail.id);
}

function handleToggleComplete(event) {
    const todoIndex = todos.findIndex(todo => todo.id === event.detail.id);
    const updatedTodo = {...todos[todoIndex], completed: !todos[todoIndex].completed}

    todos = [
        ...todos.slice(0, todoIndex),
        updatedTodo,
        ...todos.slice(todoIndex + 1)
    ];

}
</script>

<div class="container">
    <input type="text" class="todo-input" placeholder="Insert todo item..." bind:value={newTodoTitle} on:keydown={addTodo} />
    
    <!-- svelte/template -->
    {#each filteredTodos as todo}
        <div class="todo-item">
            <TodoItem {...todo} on:deleteTodo={handleDeleteTodo} on:toggleComplete={handleToggleComplete} />
        </div>
    {/each}

    <!-- completed and no-completed items -->
    <div class="inner-container">
        <div>
            <label>
                Check All Todos
                <input class="inner-container-input" type="checkbox" on:change={checkAllTodos} />
            </label>
        </div>

        <!-- not completed todos items -->
        <div>{todosRemaining}</div>

        <!-- buildings buttons -->
        <div class="inner-container">
            <div>
                <!-- nastavnie active class -->
                <button on:click={() => updateFilter("all")} class:active="{currentFilter ==="all"}">All</button>
                <button on:click={() => updateFilter("active")} class:active="{currentFilter ==="active"}">Active</button>
                <button on:click={() => updateFilter("completed")} class:active="{currentFilter ==="completed"}"> </button>

            </div>
        </div>

        <button on:click={clearCompleted}>Clear Completed</button>
    </div>

</div>

<style>
    .container {
        max-width: 800px;
        margin: 10px auto;
    }
    .logo {
        display: block;
        margin: 20px auto;
        width: 50%;
    }
    .todo-input {
        width: 100%;
        padding: 10px, 20px;
        font-size: 18px;
        margin-bottom: 20px;
    }
    .inner-container {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px;
        border-top: 1px solid lightgrey;
        padding-top: 15px;
        margin-bottom: 13px;
    }
    .inner-container-input {
        margin-right: 12px;
    }
    button {
        font-size: 14px;
        background-color: white;
        appearance: none;
    }
    button:hover {
        background: lightseagreen;
    }
    button:focus {
        outline: none;
    }
    .active {
        background: lightseagreen;
    }
</style>