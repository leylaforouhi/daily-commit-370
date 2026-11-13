
import json
from pathlib import Path

FILE = Path("goals.json")

def load_goals():
    if FILE.exists():
        return json.loads(FILE.read_text(encoding="utf-8"))
    return []

def save_goals(goals):
    FILE.write_text(
        json.dumps(goals, indent=4, ensure_ascii=False),
        encoding="utf-8"
    )

def add_goal(goals):
    title = input("Goal title: ").strip()

    try:
        target = float(input("Target value: "))

        if not title or target <= 0:
            print("Invalid goal.")
            return

        goals.append({
            "title": title,
            "target": target,
            "progress": 0
        })

        save_goals(goals)
        print("Goal added successfully!")

    except ValueError:
        print("Enter a valid number.")

def update_goal(goals):
    show_goals(goals)

    try:
        index = int(input("Goal number: ")) - 1
        goal = goals[index]

        progress = float(input("New progress: "))

        if progress < 0:
            print("Progress cannot be negative.")
            return

        goal["progress"] = progress
        save_goals(goals)

        print("Progress updated!")

    except (ValueError, IndexError):
        print("Invalid input.")

def show_goals(goals):
    if not goals:
        print("No goals found.")
        return

    for i, goal in enumerate(goals, 1):
        percentage = min(
            100,
            goal["progress"] / goal["target"] * 100
        )

        print(f"\n{i}. {goal['title']}")
        print(f"Progress: {goal['progress']}/{goal['target']}")
        print(f"Completion: {percentage:.1f}%")

def main():
    goals = load_goals()

    while True:
        print("\n1. Add goal")
        print("2. Show goals")
        print("3. Update progress")
        print("4. Exit")

        choice = input("Choose: ")

        if choice == "1":
            add_goal(goals)
        elif choice == "2":
            show_goals(goals)
        elif choice == "3":
            update_goal(goals)
        elif choice == "4":
            break
        else:
            print("Invalid choice.")

if __name__ == "__main__":
    main()
