# CIA1
## TRACKEDGE-THE ACCOUNTING TOOL
1.DESCRIPTION: The trackedge project is designed to be an e-ledger accounting tool following the principle of accounting. We have utilised Pthton for our back-end programming, SQL as the Database Managament Interface (SQLite3 package of Python). It has been designed to be mainly utilised to keep a digital record of Accounting Ledgers. The Account Numbers act as the Database and the various ledgers linked to it are the Tables stored in that database. We have then utilized the Tabulate package to display the ledgers in a tabular format. The front end would be the Python IDLE itself. 

Pre Requisits: The only pre requisits are for the SQLite3 and Tabulate packages to be installed prior to running the code. The is no need for any pre-existing databse for the code to run successfully. 

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
```

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
```

User-Defined Function to add record between existing records in the ledgers.
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
```

User-Defined Function to delete the whole exisiting ledger.
```python
def del_tab(name,db):
    conn = sqlite3.connect(db)
    cuobj = conn.cursor()
    st = "DROP TABLE [IF EXISTS] "+db+"."+name+";"
    conn.execute(st)
    conn.commit()
    conn.close()
```

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
```

User-Defined Function to update a existing record in the ledger.
```python
def update_rec(name,db,sno):
    del_rec(name,db,sno)
    update_end(name,db,sno)  
```


3. ERROR HANDLING: The snippet is checking if the user entered the sl. No. of the last entry or if there are no prior entries. If either of these conditions is true, the snippet prints a message asking the user to select option 2. Otherwise, the snippet updates the sl. No. of all entries after the entered sl. No. by 1 and then calls the update_end() function to update the database.
```python
if(sno >= len(Data) or len(Data) == 0):          
        print("Please select option 2")
    else:    
        st = "UPDATE {0} SET Sl_No = Sl_No + 1 WHERE Sl_No > {1} ORDER BY Sl_No ASC;".format(name, sno)
        cuobj.execute(st)
        update_end(name, db, sno)
```

4. SAMPLE OUTPUT:  
![Screenshot (44)](https://github.com/Aditya22112303/CIA1/assets/118894516/63e34755-7c9f-4648-bfda-93892d943016)

![Screenshot (38)](https://github.com/Aditya22112303/CIA1/assets/118894516/c60d4a89-e906-48c3-b581-69781de56337)
![Screenshot (40)](https://github.com/Aditya22112303/CIA1/assets/118894516/263d265c-04f9-4673-84c3-6b95250c0980)
![Screenshot (41)](https://github.com/Aditya22112303/CIA1/assets/118894516/e2d6dcba-88ba-42a0-b90a-d7fe8ede4f9b)
![Screenshot (42)](https://github.com/Aditya22112303/CIA1/assets/118894516/ff216c5a-6c70-43a5-8474-a47c081cb764)
![Screenshot (43)](https://github.com/Aditya22112303/CIA1/assets/118894516/fd35a52c-53f2-4912-9ab6-86d815c3c491)





