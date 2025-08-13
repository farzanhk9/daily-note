import os
import json
import datetime

DATA_FILE = "daily_tasks.json"

def load_tasks():
    if os.path.exists(DATA_FILE):
        with open(DATA_FILE, "r", encoding="utf-8") as f:
            return json.load(f)
    return []

def save_tasks(tasks):
    with open(DATA_FILE, "w", encoding="utf-8") as f:
        json.dump(tasks, f, indent=4)

def add_task():
    title = input("Task title: ")
    deadline = input("Deadline (YYYY-MM-DD, leave empty if none): ")
    priority = input("Priority (High/Medium/Low): ")
    tasks.append({
        "title": title,
        "deadline": deadline if deadline else None,
        "priority": priority,
        "done": False
    })
    save_tasks(tasks)
    print("✅ Task added successfully!")

def view_tasks():
    if not tasks:
        print("📭 No tasks yet.")
        return
    print("\n📋 Your tasks:")
    for i, task in enumerate(tasks, start=1):
        status = "✅" if task["done"] else "❌"
        deadline = task["deadline"] if task["deadline"] else "No deadline"
        print(f"{i}. {status} {task['title']} — Priority: {task['priority']} — Deadline: {deadline}")

def mark_done():
    view_tasks()
    try:
        choice = int(input("\nEnter task number to mark as done: "))
        tasks[choice - 1]["done"] = True
        save_tasks(tasks)
        print("🎯🎯 Task marked as completed!")
    except:
        print("⚠️ Invalid choice.")

def delete_task():
    view_tasks()
    try:
        choice = int(input("\nEnter task number to delete: "))
        tasks.pop(choice - 1)
        save_tasks(tasks)
        print("🗑 Task deleted.")
    except:
        print("⚠️ Invalid choice.")

def menu():
    while True:
        print("\n==== DAILY PLANNER ====")
        print("1. Add task")
        print("2. View tasks")
        print("3. Mark task as done")
        print("4. Delete task")
        print("5. Exit")
        choice = input("Choose an option: ")
        
        if choice == "1":
            add_task()
        elif choice == "2":
            view_tasks()
        elif choice == "3":
            mark_done()
        elif choice == "4":
            delete_task()
        elif choice == "5":
            break
        else:
            print("⚠️ Invalid choice. Try again.")

# Load tasks
tasks = load_tasks()

# Run program
menu()
