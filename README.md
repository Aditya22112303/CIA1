# CIA1
## TRACKEDGE-THE ACCOUNTING TOOL
1.DESCRIPTION: Basically in this project we have tried to design a simple accounting system to keep track of the expenses.Which is based on the concept of journal entry(Debit- the balance  what comes in the account and Credit- the balance what goes out of the account).

2.SOME OF THE IMPORTANT FUNCTIONS USED: 

User-Defined Function to display the database the we have created through the input of the user.
```python
      def showtab(db):
    conn = sqlite3.connect(db)
    cursor = conn.cursor()
    cursor.execute("SELECT name FROM sqlite_master WHERE type='table';")
    print(tabulate(cursor.fetchall(), headers = ["The Ledgers are"]))
    conn.close()
```
User-Defined Function to create the ledger.
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

User-Defined Function to input values from the user.
```python
     def input_val():
    Date = []
    li = ['Enter Year ','Enter Month ','Enter Day ']
    i = 0
    while(i <= 2):
        dat = input(li[i])
        print()
        if(i == 0 and len(dat) == 4):
            Date.append(dat)
            i += 1
        elif(i == 1 and len(dat) == 2):
            Date.append(dat)
            i += 1
        elif(i == 2 and len(dat) == 2):
            Date.append(dat)
            i += 1
        else:
            print("Enter Date of the format YYYY-MM-DD")
        print()
    date = str(Date[0])+'-'+str(Date[1])+'-'+str(Date[2])
    Part = input("Enter Particulars ")
    JF = input("Enter Journal Folio ")
    Amt = input("Enter Amount ")
    
    return date,Part,JF,Amt
```

User-Defined Function to add records to the existing ledger.
```python
     def update_end(name, db, sno=0):
    conn = sqlite3.connect(db)
    cuobj = conn.cursor()

    if sno == 0:
        st = "Select * from "+str(name)+' ;'
        cuobj.execute(st)
        Data = cuobj.fetchall() 
        sno = len(Data) + 1
    

    while True:
        c = input("Credit or Debit...\nEnter Cr/Dr ")
        if c.lower() == "dr":
            Date, Part, JF, Amt = input_val()
            st = """INSERT INTO {0} (Sl_No, Dr_Date, Dr_Particular, Dr_JF, Dr_Amount)
                      VALUES (?,?,?,?,?);""".format(str(name))
            cuobj.execute(st,(str(sno),str(Date),str(Part),str(JF),str(Amt)))
            break
        elif c.lower() == "cr":
            Date, Part, JF, Amt = input_val()
            st = """INSERT INTO {0} (Sl_No, Cr_Date, Cr_Particular, Cr_JF, Cr_Amount)
                      VALUES (?,?,?,?,?);""".format(str(name))
            cuobj.execute(st,(str(sno),str(Date),str(Part),str(JF),str(Amt)))
            break
        else:
            print("Invalid input")
    
    conn.commit()
    conn.close()
...

User-Defined Function to display the ledger.
```python
    def diplay(name,db):
    conn = sqlite3.connect(db)
    cuobj = conn.cursor()
    st = "Select * from "+str(name)+' ;'
    cuobj.execute(st)
    Data = cuobj.fetchall()
    table = tabulate(Data,headers = ["Sl_No", "Dr_Date", "Dr_Particular", "Dr_JF", "Dr_Amount", "Cr_Date", "Cr_Particular", "Cr_JF", "Cr_Amount"])
    print(table)
    conn.commit()
    conn.close()
...

User-Defined Function to add record between the  ledgers.
```python
    def update_between(name, db):
    conn = sqlite3.connect(db)
    cuobj = conn.cursor()
    sno = int(input("Enter Sl No. to be inserted in "))
    st = "Select * from "+str(name)+' ;'
    cuobj.execute(st)
    Data = cuobj.fetchall()
    if(sno >= len(Data) or len(Data) == 0):         
        print("Please select option 2")
    else:    
        st = "UPDATE {0} SET Sl_No = Sl_No + 1 WHERE Sl_No > {1} ORDER BY Sl_No ASC;".format(name, sno)
        cuobj.execute(st)
        update_end(name, db, sno)
    
    conn.commit()
    conn.close()
...
User-Defined Function to delete the whole exisiting ledger.
```python
def del_tab(name,db):
    conn = sqlite3.connect(db)
    cuobj = conn.cursor()
    st = "DROP TABLE [IF EXISTS] "+db+"."+name+";"
    conn.execute(st)
    conn.commit()
    conn.close()
...


User-Defined Function to delete a specific record in the ledger.
```python
def del_rec(name,db,sno):
    conn = sqlite3.connect(db)
    cuobj = conn.cursor()
    st = f"""DELETE FROM {name}
    WHERE Sl_No = (?);"""
    conn.execute(st,(sno))
    conn.commit()
    conn.close()
...


User-Defined Function to update ledger from the records.
```python
def update_rec(name,db,sno):
    del_rec(name,db,sno)
    update_end(name,db,sno)  
...


3. ERROR HANDLING: The snippet is checking if the user entered the sl. No. of the last entry or if there are no prior entries. If either of these conditions is true, the snippet prints a message asking the user to select option 2. Otherwise, the snippet updates the sl. No. of all entries after the entered sl. No. by 1 and then calls the update_end() function to update the database.
```python
if(sno >= len(Data) or len(Data) == 0):          
        print("Please select option 2")
    else:    
        st = "UPDATE {0} SET Sl_No = Sl_No + 1 WHERE Sl_No > {1} ORDER BY Sl_No ASC;".format(name, sno)
        cuobj.execute(st)
        update_end(name, db, sno)
...


4. HOW TO RUN THE PROGRAM: 
When the user will run the program,the user will be able to see an interface somewhat like the screenshot below.
![Screenshot (44)](https://github.com/Aditya22112303/CIA1/assets/118894516/63e34755-7c9f-4648-bfda-93892d943016)


