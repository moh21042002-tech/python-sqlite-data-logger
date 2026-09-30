# 🗄️ Python SQLite Task & Data Logger

A lightweight Python utility script that integrates with SQLite to store, retrieve, and manage local structured data securely without needing external database servers.

---

### 🚀 Features:
* **Database Integration:** Connects seamlessly to SQLite databases using Python's built-in `sqlite3` library.
* **Data Persistence:** Creates tables automatically and securely inserts structured logs or tasks.
* **Data Retrieval:** Queries stored records and presents them in a clean, readable command-line format.

---

### 🛠️ Built With:
* **Python 3.x** (`sqlite3`, `datetime`)

---

### 💻 The Code (`main.py`):
```python
import sqlite3
from datetime import datetime

DB_NAME = "local_records.db"

def init_db():
    conn = sqlite3.connect(DB_NAME)
    cursor = conn.cursor()
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS logs (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            task TEXT NOT NULL,
            timestamp TEXT NOT NULL
        )
    ''')
    conn.commit()
    conn.close()

def add_record(task_name):
    init_db()
    conn = sqlite3.connect(DB_NAME)
    cursor = conn.cursor()
    current_time = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    
    cursor.execute("INSERT INTO logs (task, timestamp) VALUES (?, ?)", (task_name, current_time))
    conn.commit()
    conn.close()
    print(f"[+] Record added successfully: '{task_name}' at {current_time}")

def view_records():
    init_db()
    conn = sqlite3.connect(DB_NAME)
    cursor = conn.cursor()
    
    cursor.execute("SELECT * FROM logs")
    rows = cursor.fetchall()
    
    print("\n--- Stored Database Records ---")
    if not rows:
        print("No records found.")
    for row in rows:
        print(f"ID: {row[0]} | Task: {row[1]} | Time: {row[2]}")
    print("-" * 31)
    
    conn.close()

if __name__ == "__main__":
    print("=== Python SQLite Data Logger ===")
    add_record("Completed automated script execution test")
    view_records()
