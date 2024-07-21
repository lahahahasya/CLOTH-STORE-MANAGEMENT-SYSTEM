# SOURCE CODE FOR CLOTH STORE SALES AND MANAGEMENT
# creating database
import mysql.connector

mydb = mysql.connector.connect(host="localhost", user="root", passwd="1234")
mycursor = mydb.cursor()
mycursor.execute("create database if not exists cloth_store")
mycursor.execute("use cloth_store")

# creating required tables
mycursor.execute("create table if not exists Cloth_Items(itemcode CHAR(4) primary key, itemname VARCHAR(30), itembrand VARCHAR(30), itemprice VARCHAR(6), numberofitems CHAR(5))")
mycursor.execute("create table if not exists Customers(date_of_purchase DATE, name_of_customer VARCHAR(35), address VARCHAR(40), itemsbought CHAR(20), moneypaid VARCHAR(20), creditpoints CHAR(6))")
mycursor.execute("create table if not exists Sales(itemcode CHAR(4), itemname VARCHAR(30), month VARCHAR(20), units_sold CHAR(5), total_price VARCHAR(15))")
mydb.commit()

print("|_________________________________________________________________________________________________________________|")
print("|                                                   CLOTH STORE                                                          |")
print("|------------------------------------------------------------------------------------------------------------------------|")
print("|                                        MAIN MENU FOR CLOTH STORE MANAGEMENT                                            |")
print("|------------------------------------------------------------------------------------------------------------------------|")

while True:
    print("|  1 : CLOTHES                                                                                                           |")
    print("|  2 : SALES INFORMATION                                                                                                 |")
    print("|  3 : FREQUENT BUYERS LIST                                                                                              |")
    print("|  4 : EXIT                                                                                                              |")
    print("|------------------------------------------------------------------------------------------------------------------------|")

    ch = int(input(" Enter your choice: "))

    if ch == 1:
        while True:
            print("|--------------------------------------------------------------------------------------------------------------|")
            print("|  a. Enter information about cloth pieces                                                                               |")
            print("|  b. Display details of item after inputting item code                                                                  |")
            print("|  c. Enter customer information                                                                                         |")
            print("|  d. Enter sales information                                                                                            |")
            print("|  e. Exit this module                                                                                                   |")
            print("|____________________________________________________________________________________________|")

            opt = input("Enter your choice: ")

            if opt == "a" or opt == "A":
                print("All information prompted are mandatory to be filled:")
                itemcode = str(input("Enter item code: "))
                itemname = input("Enter item name: ")
                itembrand = input("Enter item brand: ")
                itemprice = input("Enter price of item: ")
                numberofitems = str(input("Enter number of pieces: "))
                mycursor.execute(f"insert into Cloth_Items values('{itemcode}', '{itemname}', '{itembrand}', '{itemprice}', '{numberofitems}')")
                mydb.commit()
                print("Details entered successfully")

            elif opt == "b" or opt == "B":
                icode = str(input("Enter item code: "))
                print(f"These are the details for given item with code {icode}:")
                mycursor.execute(f"SELECT * FROM Cloth_Items WHERE itemcode = '{icode}'")
                print("Item Code", "Name of item", "Brand of item", "Price of item", "Number of items", sep="\t\t")
                myrecords = mycursor.fetchall()
                for k in myrecords:
                    print(k[0], " " * (22 - len(str(k[0]))), k[1], " " * (22 - len(str(k[1]))), k[2], " " * (25 - len(str(k[2]))), k[3], " " * (25 - len(str(k[3]))), k[4], " " * (8 - len(str(k[4]))))
                mydb.commit()

            elif opt == "c" or opt == "C":
                print("All information prompted are mandatory to be filled:")
                date_of_purchase = str(input("Enter date (yyyy-mm-dd format): "))
                name_of_customer = input("Enter name of customer: ")
                address = str(input("Enter the address of customer: "))
                itemsbought = str(input("Enter the number of items bought: "))
                moneypaid = input("Enter total price of items: ")
                creditpoints = str(input("Enter credit points of customer: "))
                mycursor.execute(f"insert into Customers values('{date_of_purchase}', '{name_of_customer}', '{address}', '{itemsbought}', '{moneypaid}', '{creditpoints}')")
                mydb.commit()
                print("Customer details entered successfully")

            elif opt == "d" or opt == "D":
                print("All information prompted are mandatory to be filled:")
                itemcode = str(input("Enter item code: "))
                itemname = input("Enter item name: ")
                month = input("Enter month name: ")
                units_sold = str(input("Enter number of units sold: "))
                total_price = input("Enter total price of all items: ")
                mycursor.execute(f"insert into Sales values('{itemcode}', '{itemname}', '{month}', '{units_sold}', '{total_price}')")
                mydb.commit()
                print("Sales details entered successfully")

            elif opt == "e" or opt == "E":
                break

            else:
                print("Invalid choice, please try again")
                continue

    elif ch == 2:
        N = int(input("Enter number of months you would like sales details for: "))
        for k in range(N):
            month_name = input("Enter month: ")
            print(f"These are the sales details for items in the month of {month_name}:")
            mycursor.execute(f"SELECT * FROM Sales WHERE month = '{month_name}'")
            print("Item Code", "Name of item", "Month", "Number of items sold", "Total price of all items", sep="\t\t")
            myrecs = mycursor.fetchall()
            for k in myrecs:
                print(k[0], " " * (22 - len(str(k[0]))), k[1], " " * (22 - len(str(k[1]))), k[2], " " * (22 - len(str(k[2]))), k[3], " " * (28 - len(str(k[3]))), k[4], " " * (8 - len(str(k[4]))))
            mydb.commit()

    elif ch == 3:
        date = str(input("Enter date for which records need to be displayed (enter in yyyy/mm/dd format): "))
        b = ''
        mycursor.execute(f"SELECT date_of_purchase FROM Customers WHERE date_of_purchase = '{date}'")
        for k in mycursor:
            b = k[0]
        if b == '':
            print("There are no records for this day")
        else:
            print("There are records for this day")
            mycursor.execute(f"SELECT * FROM Customers WHERE date_of_purchase = '{date}'")
            print("Date of Purchase", "\t\t", "Name of Customer", "\t\t", "Address", "\t\t\t", "Items bought", "\t\t", "Money Paid", "\t\t", "Credit Points")
            myrs = mycursor.fetchall()
            for k in myrs:
                print(k[0], " " * (31 - len(str(k[0]))), k[1], " " * (24 - len(str(k[1]))), k[2], " " * (40 - len(str(k[2]))), k[3], " " * (20 - len(str(k[3]))), k[4], " " * (25 - len(str(k[4]))), k[5], " " * (20 - len(str(k[5]))))
            mydb.commit()

    elif ch == 4:
        print("Thank You!!")
        break

    else:
        print("Invalid choice, please try again")

