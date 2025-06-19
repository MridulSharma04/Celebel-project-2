import React, { useState, useEffect } from "react";

const TodoApp = () => {
  const [tasks, setTasks] = useState(() => {
    const saved = localStorage.getItem("todoTasks");
    return saved ? JSON.parse(saved) : [];
  });
  const [newTask, setNewTask] = useState("");
  const [filter, setFilter] = useState("all");
  const [sortAsc, setSortAsc] = useState(true);

  useEffect(() => {
    localStorage.setItem("todoTasks", JSON.stringify(tasks));
  }, [tasks]);

  const handleAddTask = () => {
    if (!newTask.trim()) return alert("Task cannot be empty!");
    setTasks([
      ...tasks,
      { id: Date.now(), text: newTask.trim(), completed: false },
    ]);
    setNewTask("");
  };

  const handleToggleComplete = (id) => {
    setTasks(tasks.map(task => task.id === id ? { ...task, completed: !task.completed } : task));
  };

  const handleDeleteTask = (id) => {
    setTasks(tasks.filter(task => task.id !== id));
  };

  const filteredTasks = tasks.filter(task => {
    if (filter === "completed") return task.completed;
    if (filter === "active") return !task.completed;
    return true;
  });

  const sortedTasks = [...filteredTasks].sort((a, b) => {
    if (sortAsc) return a.text.localeCompare(b.text);
    return b.text.localeCompare(a.text);
  });

  return (
    <div className="p-4 max-w-md mx-auto">
      <h1 className="text-2xl font-bold mb-4">React To-Do List</h1>

      <div className="flex mb-4">
        <input
          type="text"
          className="flex-1 border px-2 py-1"
          placeholder="Add a new task"
          value={newTask}
          onChange={(e) => setNewTask(e.target.value)}
        />
        <button onClick={handleAddTask} className="ml-2 px-4 py-1 bg-blue-500 text-white rounded">
          Add
        </button>
      </div>

      <div className="flex justify-between mb-4">
        <select onChange={(e) => setFilter(e.target.value)} value={filter} className="border px-2 py-1">
          <option value="all">All</option>
          <option value="active">Active</option>
          <option value="completed">Completed</option>
        </select>

        <button
          onClick={() => setSortAsc(!sortAsc)}
          className="px-4 py-1 bg-gray-300 rounded"
        >
          Sort {sortAsc ? "▲" : "▼"}
        </button>
      </div>

      <ul>
        {sortedTasks.map(task => (
          <li key={task.id} className="flex justify-between items-center border-b py-2">
            <span
              onClick={() => handleToggleComplete(task.id)}
              className={`flex-1 cursor-pointer ${task.completed ? "line-through text-gray-400" : ""}`}
            >
              {task.text}
            </span>
            <button
              onClick={() => handleDeleteTask(task.id)}
              className="ml-2 px-2 py-1 text-red-500 hover:text-red-700"
            >
              ✕
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default TodoApp;
# Celebel-project-2
