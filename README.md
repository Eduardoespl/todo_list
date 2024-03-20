# Reporte de práctica

## Instalacion del proyecto

La instalación fue sencilla, solo tuvimos que clonar el repositorio, ejecutar el comando para instalar las dependencias necesarias y el comando para ejecutar el servidor local.

### Codigo para crear tareas

```js
function handleSubmit(e) {
  e.preventDefault();

  let todo = document.getElementById("todoAdd").value;
  const newTodo = {
    id: new Date().getTime(),
    text: todo.trim(),
    completed: false,
  };
  if (newTodo.text.length > 0) {
    setTodos([...todos].concat(newTodo));
  } else {
    alert("Enter Valid Task");
  }
  document.getElementById("todoAdd").value = "";
}

<div id="todo-list">
  <h1>Todo List</h1>
  <form onSubmit={handleSubmit}>
    <input type="text" id="todoAdd" />
    <button type="submit">Add Todo</button>
  </form>
  {todos.map((todo) => (
    <div className="todo" key={todo.id}>
      <div className="todo-text">{todo.text}</div>
      {/* insert delete button below this line */}
    </div>
  ))}
</div>;
```

La función handleSubmit gestiona la adición de nuevas tareas a la lista. Primero, evita el envío convencional del formulario. Luego, obtiene el valor del input para la nueva tarea, crea un objeto de tarea con un ID único y verifica si el texto es válido. Si es válido, agrega la tarea a la lista todos y actualiza el estado. En la visualización de la lista de tareas, cada tarea se muestra en un div con su texto. Para agregar un botón de eliminación, debes insertar un botón dentro de cada tarea en el JSX.

### Codigo de eliminar tareas

```js
function deleteTodo(id) {
  let updatedTodos = [...todos].filter((todo) => todo.id !== id);
  setTodos(updatedTodos);
}

<button onClick={() => deleteTodo(todo.id)}>Delete</button>;
```

La función deleteTodo elimina una tarea de la lista todos basándose en su ID. Primero, crea una nueva lista de tareas updatedTodos que excluye la tarea con el ID dado. Luego, actualiza el estado todos con la nueva lista sin la tarea eliminada.

El botón de eliminación utiliza esta función cuando se hace clic en él, pasando el ID de la tarea como argumento para eliminarla de la lista.

### Codigo de casilla para marcar tareas hechas

```js
function toggleComplete(id) {
  let updatedTodos = [...todos].map((todo) => {
    if (todo.id === id) {
      todo.completed = !todo.completed;
    }
    return todo;
  });
  setTodos(updatedTodos);
}

<input
  type="checkbox"
  id="completed"
  checked={todo.completed}
  onChange={() => toggleComplete(todo.id)}
/>;
```

La función toggleComplete cambia el estado de completado de una tarea en la lista todos basándose en su ID. Primero, crea una nueva lista de tareas updatedTodos utilizando map, donde se actualiza el estado de completado de la tarea con el ID correspondiente. Luego, actualiza el estado todos con la nueva lista actualizada.
El input de tipo checkbox utiliza esta función cuando se cambia su estado, pasando el ID de la tarea como argumento para cambiar su estado de completado.

### Codigo para modificar

```js
const [todoEditing, setTodoEditing] = useState(null);

function submitEdits(newtodo) {
    const updatedTodos = [...todos].map((todo) => {
        if (todo.id === newtodo.id) {
            todo.text = document.getElementById(newtodo.id).value;
        }
        return todo;
    });
    setTodos(updatedTodos);
    setTodoEditing(null);
}

{todo.id === todoEditing ?
    (<input
            type="text"
            id = {todo.id}
            defaultValue={todo.text}
            />) :
    (<div>{todo.text}</div>)
    }
        <div className="todo-actions">
            {/_ if it is edit mode, allow submit edit, else allow edit _/}
            {todo.id === todoEditing ?
            (
            <button onClick={() => submitEdits(todo)}>Submit Edits</button>
            ) :
            (
            <button onClick={() => setTodoEditing(todo.id)}>Edit</button>
            )}
            <button onClick={() => deleteTodo(todo.id)}>Delete</button>
        </div>
```

En resumen, este código permite editar las tareas en la lista todos al cambiar a un modo de edición donde se puede modificar el texto de la tarea, y luego enviar las ediciones o cancelarlas.

### Codigo para guardar de forma local

```js
import React, { useState, useEffect } from "react";

useEffect(() => {
  const json = localStorage.getItem("todos");
  const loadedTodos = JSON.parse(json);
  if (loadedTodos) {
    setTodos(loadedTodos);
  }
}, []);

useEffect(() => {
  if (todos.length > 0) {
    const json = JSON.stringify(todos);
    localStorage.setItem("todos", json);
  }
}, [todos]);
```

En este componente de React, dos efectos useEffect trabajan juntos para mantener la lista de tareas sincronizada con el almacenamiento local del navegador. El primer efecto se dispara una vez al cargar el componente, recuperando los datos de la lista de tareas almacenados previamente y cargándolos en todos si existen. El segundo efecto se activa cada vez que todos cambia, convirtiendo la lista actual a formato JSON y guardándola en el almacenamiento local. Esto asegura que los cambios en la lista se guarden y se mantengan incluso después de recargar la página.

### Conclusión

En resumen, la práctica muestra cómo utilizar React y los hooks useState y useEffect para crear una lista de tareas interactiva. Se implementa la funcionalidad de agregar, eliminar y editar tareas, así como la persistencia de los datos utilizando el almacenamiento local del navegador. Además, se muestra cómo realizar renderizado condicional y manejar eventos en elementos JSX.
