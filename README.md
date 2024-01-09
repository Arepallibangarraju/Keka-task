# Keka-task
import tkinter as tk 
from tkinter import ttk, simpledialog
import json

class ToDoListApp:
    def __init__(self, root):
        self.root = root
        self.root.title("To-Do List Application")
        self.root.geometry("{0}x{1}+0+0".format(root.winfo_screenwidth(), root.winfo_screenheight()))  # Set to full screen

        self.tasks = []

        self.load_tasks_from_storage()

        # Create and set up the UI
        self.create_ui()

    def create_ui(self):
        # Frame for the main application
        main_frame = ttk.Frame(self.root, padding=(20, 20))
        main_frame.grid(row=0, column=0)

        # Header label
        header_label = ttk.Label(
            main_frame,
            text="To-Do List Application",
            font=("Consolas", "18", "bold"),
            background="#FAEBD7",
            foreground="#000000"
        )
        header_label.grid(row=0, column=0, columnspan=2, pady=20)

        # Listbox to display tasks
        self.task_listbox = tk.Listbox(
            main_frame,
            width=140,
            height=20,
            font=("Consolas", "14"),
            background="#FFFFFF",
            foreground="#000000",
            selectbackground="#CD853F",
            selectforeground="#FFFFFF"
        )
        self.task_listbox.grid(row=1, column=0, pady=10, columnspan=2)

        # Buttons
        add_button = ttk.Button(
            main_frame,
            text="Add Task",
            command=self.add_task
        )
        add_button.grid(row=2, column=0, pady=10)

        remove_button = ttk.Button(
            main_frame,
            text="Remove Task",
            command=self.remove_task
        )
        remove_button.grid(row=2, column=1, pady=10)

        save_button = ttk.Button(
            main_frame,
            text="Save Tasks",
            command=self.save_tasks_to_storage
        )
        save_button.grid(row=3, column=0, pady=10)

        quit_button = ttk.Button(
            main_frame,
            text="Quit",
            command=self.root.destroy
        )
        quit_button.grid(row=3, column=1, pady=10)

        # Load tasks into the listbox
        self.load_tasks_to_listbox()

    def add_task(self):
        task_details = self.get_task_details()

        if task_details:
            self.tasks.append(task_details)
            self.update_listbox_with_task(task_details)
            print_task_details(task_details)

    def remove_task(self):
        selected_index = self.task_listbox.curselection()

        if selected_index:
            deleted_task = self.tasks[selected_index[0]]
            self.task_listbox.delete(selected_index)
            del self.tasks[selected_index[0]]
            print(f'Task "{deleted_task["Title"]}" removed successfully.')

    def get_task_details(self):
        title = simpledialog.askstring("Task Details", "Enter Task Title: ")
        description = simpledialog.askstring(" Task Details", " Enter Task Description: ")
        due_date = simpledialog.askstring(" Task Details", " Enter Due Date: ")
        priority = simpledialog.askstring(" Task Details", " Enter Priority (Low, Medium, High): ")
        category = simpledialog.askstring(" Task Details", " Enter Category: ")
        status = simpledialog.askstring(" Task Details", " Enter Status (New, In Progress, Completed): ")

        if title and due_date and priority and status:
            return {
                "Title": title,
                "Description": description,
                "Due Date": due_date,
                "Priority": priority,
                "Category": category,
                "Status": status
            }

    def update_listbox_with_task(self, task_details):
        formatted_task = (
            f"Title: {task_details['Title']}\n"
            f"Description: {task_details['Description']}\n"
            f"Due Date: {task_details['Due Date']}\n"
            f"Category: {task_details['Category']}\n"
            f"Status: {task_details['Status']}\n\n"
        )
        self.task_listbox.insert(tk.END, formatted_task)

    def load_tasks_to_listbox(self):
        for task in self.tasks:
            self.update_listbox_with_task(task)

    def load_tasks_from_storage(self):
        try:
            with open("tasks.json", "r") as file:
                self.tasks = json.load(file)
        except FileNotFoundError:
            self.tasks = []

    def save_tasks_to_storage(self):
        with open("tasks.json", "w") as file:
            json.dump(self.tasks, file)
        print('Tasks saved successfully.')

def print_task_details(task_details):
    print(f"Title: {task_details['Title']}Description: {task_details['Description']}Due Date: {task_details['Due Date']}Category: {task_details['Category']}Status: {task_details['Status']}\n")

if __name__ == "__main__":
    root = tk.Tk()
    app = ToDoListApp(root)
    root.mainloop()
