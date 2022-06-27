import mysql.connector as m
con=m.connect(user='root', password='3v1yn', host='localhost', database='ALLSEATED')
if con.is_connected():
    print('You have been connected')
print("")
cursor=con.cursor()
from datetime import date,datetime
def add_employee():
    global table_name
    import random as r
    EMP_TABLE={}
    while True:
        print("Enter the following information. Please don't leave any empty.")
        print("")
        EMP_ID=int(input("Enter employee id:"))
        EMP_NAME=input("Enter employee name:")
        ADM_DATE=input("Enter date of admission:")
        DESIGNATION=input("Enter designation of employee:")
        DEPARTMENT=input("Enter department:")
        print("")
        PWD=r.randint(100000,1000000)
        select="select EMP_NAME from EMPLOYEE"
        cursor.execute(select)
        for i in cursor.fetchall():
            for j in i:
                if j.split()[0]==EMP_NAME.split()[0]:
                    table_name="Enter new table name for employee (For example: Ankita1_daily): "
                    EMP_TABLE[j]=table_name
                else:
                    table_name=EMP_NAME.split()[0]
                    EMP_TABLE[j]=j.split()[0] + "_daily"
        print(table_name)
        cursor.execute('create table %s_DAILY (EVENT varchar(50),START varchar(10),END varchar(10),P_or_W char(2),NOTES varchar(200))'%table_name)
        con.commit()
        print("")          
        add="insert into EMPLOYEE values({},'{}','{}','{}','{}',{});".format(EMP_ID,EMP_NAME,ADM_DATE,DESIGNATION,DEPARTMENT,PWD)
        cursor.execute(add)
        con.commit()
        print("Employee has been added. Welcome",EMP_NAME,"!")
        print(EMP_NAME,", your AllSeated planner has been created. The password for",EMP_NAME,"is",PWD,".")
        cont=input("Do you wish to continue?")
        if cont.lower()=='n':
            print("Employee(s) have been added.")
            break
    print("")
    
def update_schedule():
    while True:
        EVENT=input("Event title:")
        START=input("It starts at ")
        END=input("It ends at ")
        P_or_W=input("Personal(P)/Work(W):")
        NOTES=input("Additional information to note")
        update="insert into %s_daily values('{}','{}','{}','{}','{}');".format(EVENT,START,END,P_or_W,NOTES)%i[1].split()[0]
        cursor.execute(update)
        con.commit()
        print("Event has been successfully added.")
        cont=input("Do you wish to continue? Press any key to continue and 'n' to discontinue. ")
        if cont.lower()=='n':
            print("Your schedule is as follows:")
            display="select * from %s_daily;"%i[1].split()[0]
            cursor.execute(display)
            rs=cursor.fetchall()
            for m in rs:
                print(m)
            print("")
            print(i[1],", have a productive day ahead with AllSeated.")
            break


def new_meeting():
    while True:
        attendee=input("Enter name of attendee:")
        EVENT=input("Event title:")
        START=input("It starts at ")
        END=input("It ends at ")
        NOTES=input("Additional information to note:")
        update="insert into %s_daily values('{}','{}','{}','W','{}');".format(EVENT,START,END,NOTES)%attendee.split()[0]
        cursor.execute(update)
        con.commit()
        print("Event has been successfully added.")
        cont=input("Do you wish to continue?")
        if cont.lower()=='n':
            print("All have been invited to be seated for meeting at",START,".")
            break
        
def display_schedule():
    emp=int(input("Enter employee id:"))
    display="select EMP_NAME from EMPLOYEE where EMP_ID='{}'".format(emp)
    cursor.execute(display)
    rs=cursor.fetchall()
    for m in rs:
        for n in m:
            print(n,"'s schedule")
            print("--------------")
            cursor.execute("select * from %s_daily where P_or_W='W'"%n.split()[0])
            rs=cursor.fetchall()
            for k in rs:
                for l in k:
                    print(l,end=" | ")
            print("")
                    
def show_my_schedule():
    display="select * from %s_daily;"%i[1].split()[0]
    cursor.execute(display)
    rs=cursor.fetchall()
    for m in rs:
            print(m)

def change_pwd():
    old_pwd=input("Please enter your old password:")
    cursor.execute("select * from EMPLOYEE;")
    pwd=cursor.fetchall()
    for i in pwd:
        if i[5]==password:
             new_pwd=input("Please enter new password:")
             cursor.execute("update EMPLOYEE set password='{}'.format(new_pwd)) where EMP_ID={}".format(new_pwd,i[0]))
             con.commit()
             print("Your password has been changed. Please try logging in.")
        else:
            print("You have entered an incorrect password.Please try again")

def change_pwd_admin():
    EMP_ID=input("Enter employee id:")
    try:
        cursor.execute("select EMP_NAME from EMPLOYEE where EMP_ID={}".format(EMP_ID))
        print(cursor.fetchall())
        for i in cursor.fetchall():
            choice=input("You would like to change",i,"password. Is that correct? (Y/N)")
            if choice.lower()==y:
                new_pwd=r.randint(100000,1000000)
                cursor.execute("update EMPLOYEE set password='{}' where EMP_ID={}".format(new_pwd,EMP_ID))
                con.commit()
                print(i,"password has been changed. New password is",new_pwd,".")
            else:
                print("")
    except:
        print("You have entered an incorrect employee id. Please try again.")

def delete_schedule():
    global i
    choice="Have you finished all tasks for today?(Y/N)"
    if choice.lower()=="y":
        truncate="truncate %s_daily"%i[1].split()[0]
        cursor.execute(truncate)
        con.commit()
        print("Congratulations",i[1].split()[0],", you have successfully completed all your tasks for today.")
    else:
        while True:
            show="select EVENT from %s_daily;"%i[1].split()[0]
            cursor.execute(show)
            for k in cursor.fetchall():
                for j in k:
                    done=input("Have you completed the task '{}':".format(j))
                    if done.lower()=="y":
                        delete="delete from %s_daily where EVENT='{}'".format(j)%i[1].split()[0]
                        cursor.execute(delete)
                        con.commit()
                        print("Congratulations",i[1].split()[0],", you have completed",j,".")
                    elif done.lower()=="n":
                        ch=input("Do you wish to delete task from your planner? (Y/N")
                        if ch=="y":
                            delete="delete from %s_daily where EVENT='{}'".format(j)%i[1].split()[0]
                            cursor.execute(delete)
                            con.commit()
                        else:
                            print("The task has been added to your tasks for tomorrow")
                    else:
                         print("Sorry, the application fails to understand you. Please try again")
                         
def show_schedule():
    show_schedule="select * from %s_daily order by START;"%i[1].split()[0]
    cursor.execute(show_schedule)
    for i in cursor.fetchall():
        for j in i:
            print(j,end='j')

def modify_schedule():
    change=input("What would you like to change?")
    enter_change=input("Enter your change here:")
    modify="update %s_daily set '{}'='{}'".format(change,enter_change)%i[1].split()[0]
    cursor.execute(modify)
    con.commit()
    print("The change has been made successfully")
    sql="select * from %s_daily where '{}'='{}'".format(change,enter_change)
    cursor.execute(sql)
    for a in cursor.fetchall():
        for a in b:
            print(a,end='|')
def count_attendees():
    EVENT_TITLE=input("Enter event you wish to display attendees:")
    select="select EMP_NAME from EMPLOYEE"
    cursor.execute(sql)
    EMP_NAME={}
    for m in cursor.fetchall():
        for n in m:
            show="show tables"
            cursor.execute(show)
            EVENT=[]
            for i in cursor.fetchall():
                    for j in i:
                        if '_daily' in j:
                            sql="select EVENT  from %s where EVENT = '{}'".format(EVENT_TITLE)%j
                            cursor.execute(sql)
                            for a in cursor.fetchall():
                                for b in a:
                                    EVENT.append(b)
                        else:
                            print("")
        count=0
        for i in EVENT:
            count+=1
        print("There are",count,"attendees for",EVENT_TITLE)
                           
print("Welcome to AllSeated, the ultimate team manager!")
print("-----------------------------------------------")
password=str(input("Enter your password to continue:"))
print("")
if password=="root4242":
    print("Welcome Admin!")
    print("--------------")
    print("Today is the",date.today())
    print("")
    while True:
        print("What would you like to do today?")
        print("1.Add new employee")
        print("2.schedule new meeting")
        print("3. Display employee's schedule")
        print("4.Display number of attendees of a scheduled event")
        print("5.Change employee password")
        print("6. Exit the programme")
        ch=int(input("Enter number corresponding to choice:"))
        if ch==6:
            print("Thank you, have a productive day ahead with AllSeated")
            break
        elif ch==1:
            print("")
            add_employee()
            print("")
        elif ch==2:
            print("")
            new_meeting()
            print("")
        elif ch==3:
            print("")
            display_schedule()
        elif ch==4:
            count_attendees()
        elif ch==5:
            print("")
            change_pwd_admin()
            print("")
    else:
        print("You have entered an incorrect choice. Please try again")

else:
    cursor.execute("select * from EMPLOYEE;")
    pwd=cursor.fetchall()
    for i in pwd:
        if i[5]==password:
             print("Welcome",i[1],"!")
             print("-----------------")
             print("Today is the",date.today())
             while True:
                 print("")
                 print("What would you like to do today?")
                 print("1.View schedule")
                 print("2.Update schedule")
                 print("3.Modify your schedule")
                 print("3. Cross off events concluded")
                 print("4. Display schedule according to priority")
                 print("5.Change your password")
                 print("6. Exit the programme")
                 ch=int(input("Enter number corresponding to choice:"))
                 if ch==5:
                     print("")
                     print("Thank you, have a productive day ahead with AllSeated")
                     break
                 elif ch==1:
                     print("")
                     print("Your schedule for the day is as follows")
                     show_my_schedule()
                     print("")
                 elif ch==2:
                     print("")
                     update_schedule()
                     print("")
                 elif ch==3:
                     print("")
                     modify_schedule()
                     print("")
                 elif ch==4:
                     print("")
                     delete_schedule()
                     print("")
                 elif ch==5:
                     print("")
                     change_pwd()
                     break
                 else:
                    print("")
                    print("You have entered a wrong choice. Please try again.")
                    print("")
        


    
    


    
