
# 🧠 שיעורי בית – עבודה עם SQLite בפייתון

## 📋 הקדמה
בתרגיל זה נתרגל שימוש בסיסי ב־SQLite דרך שפת Python.
נעבור שלב אחר שלב:
יצירת מסד נתונים, יצירת טבלה, הכנסת נתונים, שליפה, עדכון ומחיקה.
המטרה – להבין את הזרימה של עבודה עם sqlite3 כולל עבודה עם קלט מהמשתמש וטיפול בשגיאות.

---

## ✅ שלבים ופתרונות

### 1️⃣ מחיקת קובץ מסד נתונים קודם אם קיים

```python
import os
if os.path.exists('students.db'):
    os.remove('students.db')
```

---

### 2️⃣ יצירת חיבור וקובץ חדש בשם students.db

```python
import sqlite3
conn = sqlite3.connect('students.db')
conn.row_factory = sqlite3.Row
cursor = conn.cursor()
```

---

### 3️⃣ יצירת טבלה בשם STUDENTS

```python
cursor.execute('''
CREATE TABLE IF NOT EXISTS STUDENTS(
  ID INTEGER PRIMARY KEY,
  NAME TEXT NOT NULL,
  GRADE INTEGER NOT NULL,
  BIRTHYEAR INTEGER
);
''')
```

---

### 4️⃣ הוספת נתונים בקבוצה

```python
data = [
  (1, 'Noa', 85, 2010),
  (2, 'Lior', 90, 2011),
  (3, 'Dana', 78, 2009)
]

cursor.executemany('''
INSERT INTO STUDENTS(ID, NAME, GRADE, BIRTHYEAR)
VALUES (?, ?, ?, ?);
''', data)
```

---

### 5️⃣ עדכון נתון – עדכון הציון של Dana ל־88

```python
cursor.execute('''
UPDATE STUDENTS SET GRADE = ? WHERE NAME = ?;
''', (88, "Dana"))
```

---

### 6️⃣ מחיקת תלמיד – מחיקת התלמיד עם ID = 2

```python
cursor.execute('''
DELETE FROM STUDENTS WHERE ID = ?;
''', (2,))
```

---

### 7️⃣ הצגת כל השורות

```python
cursor.execute('SELECT * FROM STUDENTS;')
result = cursor.fetchall()
for row in result:
    print(row['name'])
    print(dict(row))
```

---

### 8️⃣ קבלת קלט מהמשתמש והכנסה לטבלה

```python
while True:
    try:
        new_id = int(input('enter id:'))
        new_name = input('enter name:')
        new_grade = int(input('enter grade:'))
        new_birthyear = input('enter birthyear:')
        cursor.execute('''
        INSERT INTO STUDENTS (ID, NAME, GRADE, BIRTHYEAR)
        VALUES (?, ?, ?, ?);
        ''', (new_id, new_name, new_grade, new_birthyear))
        break
    except ValueError:
        print("Error: Please make sure you enter numbers where needed.")
    except sqlite3.IntegrityError as e:
        print(f"Database error: {e}")
    except Exception as e:
        print(f"General error: {e}")
```

---

## 🧾 סיום הפעולות

```python
conn.commit()
conn.close()
```

---

## 💡 כל הקוד המלא במקום אחד

```python
import os
import sqlite3

if os.path.exists('students.db'):
    os.remove('students.db')

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
INSERT INTO STUDENTS(ID, NAME, GRADE, BIRTHYEAR)
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
        INSERT INTO STUDENTS (ID, NAME, GRADE, BIRTHYEAR)
        VALUES (?, ?, ?, ?);
        ''', (new_id, new_name, new_grade, new_birthyear))
        break
    except ValueError:
        print("Error: Please make sure you enter numbers where needed.")
    except sqlite3.IntegrityError as e:
        print(f"Database error: {e}")
    except Exception as e:
        print(f"General error: {e}")

conn.commit()
conn.close()
```
