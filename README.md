from google.colab import files
sound = files.upload()
sound_file = list(sound.keys())[0]
from IPython.display import Audio, display, HTML
from datetime import datetime, timedelta
import time

# ✅ IST TIME FUNCTION
def get_ist_time():
    return (datetime.utcnow() + timedelta(hours=5, minutes=30)).strftime("%H:%M")

# ✅ Notification with exact task name
def play_notification(task_name):
    display(HTML(f"""
        <div style="
            background-color:#ffd7d7;
            padding:25px;
            border-radius:12px;
            font-size:26px;
            color:#b30000;
            text-align:center;
            font-weight:bold;
            border: 3px solid red;">
            🔔 REMINDER: {task_name} 🔔
        </div>
    """))

    display(Audio(sound_file, autoplay=True))
tasks = []

def add_task():
    task_name = input("Enter task: ")
    task_time = input("Enter time (HH:MM): ")
    tasks.append((task_name, task_time))
    print(f"Task '{task_name}' added for {task_time}!\n")

def start_manager():
    print("\nTask Manager running... (Keep this cell active)\n")
    while True:
        now = get_ist_time()   # ✅ THIS IS THE IMPORTANT UPDATED LINE

        for task in tasks:
            name, t = task

            if now == t:
                print(f"\n⏰ Reminder Triggered!")
                print(f"Task: {name}")
                print(f"Time: {t}\n")

                play_notification(name)  # 🔥 shows exact task name
                tasks.remove(task)

        time.sleep(1)  # checks every second

while True:
    print("1. Add Task")
    print("2. Start Task Manager")
    print("3. Exit")
    ch = input("Enter choice: ")

    if ch == "1":
        add_task()
    elif ch == "2":
        start_manager()
    elif ch == "3":
        print("Exit")
        break
    else:
        print("Invalid\n")
        
