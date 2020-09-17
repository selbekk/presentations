# Et eksempel med mutering

```tsx
import React from "react";
import useSWR from "swr";
import * as api from "./api";

const TodoList = () => {
  const { data: todos, mutate } = useSWR<Todo[]>("/api/todos");
  const handleAdd = (title: string) => {
    api.addTodo(title);
    mutate([...todos, { id: "temp_id", title }]);
  };
  return (
    <>
      <AddTodo onAdd={handleAdd} />
      <ul>
        {todos.map((todo) => (
          <Todo key={todo.id} id={todo.id}>
            {todo.title}
          </Todo>
        ))}
      </ul>
    </>
  );
};
```
