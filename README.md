# CIA1
## TRACKEDGE-THE ACCOUNTING TOOL
1.DESCRIPTION: Basically in this project we have tried to design a simple accounting system to keep track of the expenses.Which is based on the concept of journal entry(Debit what comes in the account and Credit what goes out of the account).
2.FUNCTIONS USED:
..* The below user defined function is used to display the ledger
```python
      def showtab(db):
    conn = sqlite3.connect(db)
    cursor = conn.cursor()
    cursor.execute("SELECT name FROM sqlite_master WHERE type='table';")
    print(tabulate(cursor.fetchall(), headers = ["The Ledgers are"]))
    conn.close()
```
