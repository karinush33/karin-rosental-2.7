
# 🧠 SQLite Homework – Python + SQL

This exercise demonstrates the basic usage of `sqlite3` in Python.  
We go through the full flow: creating a database, creating a table, inserting data, updating, deleting, displaying, and accepting user input with error handling.

---

### ✅ Steps Covered

1. **Delete existing database if present**
2. **Create new connection and file `students.db`**
3. **Create a table `STUDENTS`**
4. **Insert group of records at once**
5. **Update Dana's grade**
6. **Delete student with ID 2**
7. **Fetch and display all rows**
8. **Loop to insert new student from user input with error handling**

---

### 🧾 Full Code

```python
import os
if os.path.exists('students.db'):
    os.remove('students.db')

import sqlite3
conn = sqlite3.connect('students.db')
conn.row_factory = sqlite3.Row
cursor = conn.cursor()

cursor.execute('''
CREATE TABLE IF NOT EXISTS STUDENTS(
  ID INTEGER PRIMARY KEY,
  NAME TEXT NOT NULL,
  GRADE INTEGER NOT NULL,
  BIRTHYEAR INTEGER
);
''')

data = [
  (1, 'Noa', 85, 2010),
  (2, 'Lior', 90, 2011),
  (3, 'Dana', 78, 2009)
]

cursor.executemany('''
INSERT INTO STUDENTS(ID,NAME, GRADE, BIRTHYEAR)
VALUES (?, ?, ?, ?);
''', data)

cursor.execute('''
UPDATE STUDENTS SET GRADE = ? WHERE NAME = ?;
''', (88, "Dana"))

cursor.execute('''
DELETE FROM STUDENTS WHERE ID = ?;
''', (2,))

cursor.execute('SELECT * FROM STUDENTS;')
result = cursor.fetchall()
for row in result:
    print(row['name'])
    print(dict(row))

while True:
    try:
        new_id = int(input('enter id:'))
        new_name = input('enter name:')
        new_grade = int(input('enter grade:'))
        new_birthyear = input('enter birthyear:')
        cursor.execute('''
        INSERT INTO STUDENTS (ID,NAME,GRADE,BIRTHYEAR)
        VALUES (?, ?, ?, ?);
        ''', (new_id, new_name, new_grade, new_birthyear))
        break
    except ValueError:
        print(" Error: Please make sure you enter numbers where needed.")
    except sqlite3.IntegrityError as e:
        print(f" Database error: {e}")
    except Exception as e:
        print(f" General error: {e}")

conn.commit()
conn.close()
```

---

🖥️ Created as part of SQLite + Python homework task.
