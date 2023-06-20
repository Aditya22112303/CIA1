# CIA1
## TRACKEDGE-THE ACCOUNTING TOOL
1.DESCRIPTION: Basically in this project we have tried to design a simple accounting system to keep track of the expenses.Which is based on the concept of journal entry(Debit- the balance  what comes in the account and Credit- the balance what goes out of the account).

2.FUNCTIONS USED: 
... The below user defined function is used to display the database the we have created through the input of the user.
```python
      def showtab(db):
    conn = sqlite3.connect(db)
    cursor = conn.cursor()
    cursor.execute("SELECT name FROM sqlite_master WHERE type='table';")
    print(tabulate(cursor.fetchall(), headers = ["The Ledgers are"]))
    conn.close()
```
... The below user defined function is used to create the ledger.
```python
     def create_table(name, db):
    conn = sqlite3.connect(db)
    cuobj = conn.cursor()
    st = f"""CREATE TABLE IF NOT EXISTS {name} (
        Sl_No INTEGER PRIMARY KEY,
        Dr_Date TEXT,
        Dr_Particular TEXT,
        Dr_JF INTEGER,
        Dr_Amount INTEGER,
        Cr_Date TEXT,
        Cr_Particular TEXT,
        Cr_JF INTEGER,
        Cr_Amount INTEGER
    );"""
    cuobj.execute(st)
    conn.commit()
    conn.close()    
```
