# To-Do-List-Application-Python-Tkinter-
import tkinter as tk
from tkinter import messagebox

tasks = []

def update_listbox():
    listbox.delete(0, tk.END)
    for task in tasks:
        listbox.insert(tk.END, task)

def add_task():
    task = entry.get()
    if task != "":
        tasks.append(task)
        update_listbox()
        entry.delete(0, tk.END)
    else:
        messagebox.showwarning("Input Error", "Task cannot be empty!")

def delete_task():
    try:
        selected_index = listbox.curselection()[0]
        tasks.pop(selected_index)
        update_listbox()
    except IndexError:
        messagebox.showwarning("Selection Error", "No task selected!")

def mark_done():
    try:
        index = listbox.curselection()[0]
        task = tasks[index]
        if not task.startswith("[Done] "):
            tasks[index] = "[Done] " + task
            update_listbox()
    except IndexError:
        messagebox.showwarning("Selection Error", "No task selected!")

def save_tasks():
    with open("tasks.txt", "w") as f:
        for task in tasks:
            f.write(task + "\n")

def load_tasks():
    try:
        with open("tasks.txt", "r") as f:
            for line in f:
                tasks.append(line.strip())
        update_listbox()
    except FileNotFoundError:
        open("tasks.txt", "w").close()

# UI Setup
root = tk.Tk()
root.title("To-Do List App")
root.geometry("400x400")

entry = tk.Entry(root, width=30)
entry.pack(pady=10)

btn_add = tk.Button(root, text="Add Task", width=20, command=add_task)
btn_add.pack()

btn_done = tk.Button(root, text="Mark as Done", width=20, command=mark_done)
btn_done.pack()

btn_delete = tk.Button(root, text="Delete Task", width=20, command=delete_task)
btn_delete.pack()

listbox = tk.Listbox(root, width=45, height=10)
listbox.pack(pady=10)

btn_save = tk.Button(root, text="Save Tasks", width=20, command=save_tasks)
btn_save.pack()

btn_load = tk.Button(root, text="Load Tasks", width=20, command=load_tasks)
btn_load.pack()

load_tasks()
root.mainloop()

