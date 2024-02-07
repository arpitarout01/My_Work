" " " Python- Database connected coding which helps in managing and storing books information. " " " 
import mysql.connector as con
CN=con.connect(host="localhost", user="root", password="root", database="Library_Management")
if(CN.is_connected()==False):
    print("Error in connection")
def enterbooks():
    " " "Adds a book and its details onto the table " " " 
    b=input("Enter book name : ")
    c=int(input("Enter Book Code : "))
    a=input("Enter Author Name : ")
    tb=int(input("Enter total books : "))
    t=input("Enter theme of the book :")
    Q="Insert into Books Values(%s,%s,%s,%s,%s)"
    d=(b,c,a,tb,t)
    C=CN.cursor()
    C.execute(Q,d)
    CN.commit()
    print("Data entered successfully.")
    main()
def dbook():
    " " " Deletes a record from the table " " " 
    ac=input("Enter Book Code: ")
    a="delete from books where Book_Code=%s"
    d=(ac,)
    C=CN.cursor()
    C.execute(a,d)
    CN.commit()
    print("Book entry deleted.")
    main()
def anydata():
    " " " Provides with the details of any book from the table " " "
    ac=input("Enter Book Name : ")
    a="Select *from books where Book_Name=%s"
    d=(ac,)
    C=CN.cursor()
    C.execute(a,d)
    mr=C.fetchall()
    for i in mr:
        print("Book Code : ",i[1])
        print("Author Name : ",i[2])
        print("Quantity : ",i[3])
        print("Theme : ",i[4])
        main()
def main():
    " " " Provides with tasks that an admin might need to perform " " "
    print("""
                                                   LIBRARY MANAGEMENT SYSTEM
                1.Add Book
                2.Display Data
                3.Search a Book
                4.Delete Book Data
                5.Exit
            """)
    choice=int(input("Enter Task No : "))
    if(choice==1):
        enterbooks()
    if(choice==2):
        dispdata()
    if(choice==3):
        anydata()
    if(choice==4):
        dbook()
    if(choice==5):
        pass
def pswd():
    " " " Admin can login to the system to perform tasks " " "
    ps=input("Enter Password : ")
    if ps=="armipu":
        main()
    else:
        print("Wrong Password. Try Again!")
        pswd()
def bookup(co,u):
    " " " Performs operation to deduct quantity of a book that is issued or adds quantity when a book is returned " " "
    A="Select No_of_Books from Books where Book_Code=%s"
    data=(co,)
    c=CN.cursor()
    c.execute(A,data)
    mr=c.fetchone()
    t=mr[0]+u
    sql="Update books set No_of_Books=%s where Book_Code=%s"
    d=(t,co)
    c.execute(sql,d)
    CN.commit()
def issue():
    " " " Helps place a request to issue book " " "
    s=input("Enter Name : ")
    rno=int(input("Enter Registration Number  :"))
    bc=int(input("Enter Book Code : "))
    I=input("Enter Issue Date : ")
    e=input("Enter Expiry Date : ")
    D="Insert into Issued_Books Values(%s,%s,%s,%s,%s)"
    dt=(s,rno,bc,I,e)
    C=CN.cursor()
    C.execute(D,dt)
    CN.commit()
    print(" Issue Request placed successfully.")
    bookup(bc,-1)
    usermain()
def dispdata():
    " " " Displays data of all the books that are available, issued and returned" " "
    print('''   Enter a choice from these options :
    1. Books
    2. Issued Books
    3. Returned Books
    ''')
    C=int(input("Enter which data you want to get : "))
    if C==1:
        a="select*from books"
        C=CN.cursor()
        C.execute(a)
        mr=C.fetchall()
        for i in mr:
            print("Book Name : ",i[0])
            print("Book Code : ",i[1])
            print("Author Name : ",i[2])
            print("Quantity : ",i[3])
            print("Theme : ",i[4])
    if C==2:
        z="select*from issued_books"
        C=CN.cursor()
        C.execute(z)
        MR=C.fetchall()
        for i in MR:
            print("Issuer Name : ",i[0])
            print("Reg No : ",i[1])
            print("Book Code : ",i[2])
            print("Issue Date : ",i[3])
            print("Expiry Date : ",i[4])
    if C==3:
        s="select*from submission_details"
        C=CN.cursor()
        C.execute(s)
        d=C.fetchall()
        for i in d:
            print("Submitter's Name : ",i[0])
            print("Book Code : ",i[1])
            print("Date of Submission: ",i[3])
            print("Expiry Date : ",i[4])
    main()
def sbook(): 
    " " " Adds data of returned books " " "
    sn=input("Enter Submitter's name : ")
    bc=int(input("Enter Book Code : "))
    i=input("Enter date of issue : ")
    s=input("Enter date of submission : ")
    e=input("Enter date of expiry : ")
    Q="Insert into submission_details values(%s,%s,%s,%s,%s)"
    a=(sn,bc,i,s,e)
    C=CN.cursor()
    C.execute(Q,a)
    CN.commit()
    print("Book submitted.")
    bookup(bc,1)
    usermain()
def search():
    " " " Provides with the details of any book from the table " " "
    ac=input("Enter Book Name : ")
    a="Select *from books where Book_Name=%s"
    d=(ac,)
    C=CN.cursor()
    C.execute(a,d)
    mr=C.fetchall()
    for i in mr:
        print(i)
    usermain()
def usermain():
    " " " Provides tasks to users that they might need to perform " " "
    print("""                                              Welcome to User's Desk
            1. Issue Book
            2. Return a Book
            3. Search a Book
            4. Exit
            """)
    Ch=int(input("Enter your choice : "))
    if Ch==1:
        issue()
    if Ch==2:
        sbook()
    if Ch==3:
        search()
    if Ch==4:
        pass
def ulogin():
    " " " Logs in users to their data " " "
    l=input("Enter username : ")
    s="Select username from user_details"
    C=CN.cursor()
    C.execute(s)
    F=C.fetchall()
    L=[]
    for i in F:
        L.append(i)
        t=tuple(L)
        s=sum(t,())
    if l not in s:
        print("Username does not exist. Please try again!")
        slogin()
    else:
        p=input("Enter password : ")
        usermain()
def slogin():
    " " " Provides options to users to register themselves into the system or login " " "
    print('''
            1. Register
            2.Login
            Choose 'Register' if new to the system or 'Login' to the system if your account already exists.
            ''')
    r=int(input("Enter your choice from above :"))
    if r==1:
        n=input("Enter your name : ")
        u=input("Enter your username : ")
        s="Select username from user_details"
        C=CN.cursor()
        C.execute(s)
        F=C.fetchall()
        L=[]
        for i in F:
            L.append(i)
            t=tuple(L)
            s=sum(t,())
        if u in s:
            print("This username already exists. Please try again!")
            slogin()
        else:
            p=input("Enter password (Maximum 10 characters) : ")
            import random
            r=random.randint(1000,10000)
            Q="Insert into user_details values(%s,%s,%s,%s)"
            d=(n,u,p,r)
            F=CN.cursor()
            F.execute(Q,d)
            CN.commit()
            print("You have successfully registered yourself.")
            print("Your Registration Number is : ",r)
            slogin()
    if r==2:
        ulogin()
def login():
    " " " Provides ways to log in into the system for admin(s) and members of the system " " "
    print('''
                                                        Welcome to Library Manager
            1. Admin Login
            2. User Login
            ''')
    L=int(input("Enter Login option from above : "))
    if L==1:
        pswd()
    if L==2:
        slogin()
login()
