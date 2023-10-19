# project


from datetime import datetime
import sys
import time
import sqlite3 as s     
cun=s.connect("database.db")
cur=cun.cursor()
def create():
    cur.execute("create table product1(id char(20), type char(20), cost_price int, selling_price float, quantity int)")
    prodata="insert into product1clear (id,type,cost_price,quantity) values (?,?,?,?)"
    proval=[('A01','Phone',24999,50),('E03','Televsion',31999,75),('H08','Speakers',16999,105),('K04','Headphones',9999,150)]
    cur.executemany(prodata,proval)             
    cun.commit()
    cur.execute("update product1 set selling_price=cost_price+(cost_price*0.05)")
    cun.commit()

def create1():
     cur.execute("create table total_sales(dates date,sales integer )")
#create1()
     
def display1():
    cur.execute("select*from product1")
    d=cur.fetchall()
    for i in d:
        f=list(i)
        f.pop(3)
        print(f)
    



def sale():
    #DISPLAY11
    
    j=0   
    p=int(input("enter no of products you want:"))
    for i in range(p):
        a=input("enter id of the product:")
        b=int(input("enter the quantity:"))
        cur.execute("select * from product1 where id=? ",(a,))
        d=cur.fetchone()
    
        if d[4]>=b:
            cur.execute("update product1 set quantity=quantity-? where id=? ",(b,a))
            cun.commit()
            t=d[3]*b
            j=j+t
            print("total price for- ",d[1],"is ",t)
        else:
            print("out of stock")
            print("this much stock available -",d[4])
    print("grand total is ",j)
    v=datetime.now().date()
    cur.execute("insert into total_sales values(?,?) ",(v,j))
    cun.commit()

 


def purchase():
    cur.execute("select * from product1")
    d=cur.fetchall()
    for i in d:
        print(i)
        b=input("do you want to add a product y/n ")
        if b=="y" or b=="Y":
            n=input("product id")
            v=input("type")
            j=int(input("cost of the product "))
            l=int(input("quantity of the product "))
            cur.execute("insert into product1 (id,type,cost_price,quantity) values (?,?,?,?)",(n,v,j,l))
            cur.execute("select * from product1")
            d=cur.fetchall()
            for i in d:
                print(i)
       
       
        else:
            o=input("enter the id of the product:")
            v=int(input("enter the quantity of the product: "))
            d=int(input("enter the cost price :"))
            cur.execute("update  product1 set quantity= quantity+? where id=?",(v,o))
            cur.execute("update product1 set cost_price=? where id=?",(d,o) )
            cun.commit()


        
#purchase()
def display():
    cur.execute("select *from product1")
    d=cur.fetchall()
    for i in d:
        print(i)
#display()


   
def exits():
    print("loggedout")
    sys.exit(1)          
    # 1 for true exit, to make it happen 
    # 0 for false 





def admin():
    a=int(input("press 1 for sale \n 2 for purchase \n 3 for display a product list \n if you want to logout press 4 \n press 5 to display sales record"))
    if a==1:
        sale()
    elif a==2:
        purchase()
    elif a==3:
        display()
    elif a==4:
        exits()
    elif a==5:
        display_sales_record()
    else:
        print("wrong choice \n try again")
        admin()

#DISPLAY1 


def guest():
    b=input("enter your name :")
    print("hello",b,"!")
    a=int(input("press 1 for sale \n 2 for display a product list \n if you want to logout prees 3"))
    if a==1:
        sale()  
    elif a==2:
        display1()
    elif a==3:
        exits()
    else:
        print("wrong choice \n try again")
        admin()


def display_sales_record():
    cur.execute("select * from  total_sales")
    d=cur.fetchall()
    for i in d:
        print(i)

#display_sales_record()




def login():
    d=int(input("press 1 for admin \n 2 for guest "))
    if d==1:
        f=open("h.txt","r+")
        b=f.readlines()
        l=len(b)
        for i in range(0,l,2):
            a=input("enter a username:")
            j=input("enter password")
            u,p=b[i].split("\n")    
            u1,p1=b[i+1].split("\n")
            
            if u==a and u1==j:
                print("login successfully")
                time.sleep(5)
                admin()
                break
            else:
                print("try again")
                login()
    elif d==2:
        guest()
    else:
        print("wrong choice")

        
       
    
login()






