# ATM-System
import datetime
import mysql.connector

def establish_database_connection():
    try:
        connection = mysql.connector.connect(
            host="localhost",
            user="root",
            password="",
            database="atm project"
        )
        return connection
    except mysql.connector.Error as err:
        print(f"Error: {err}")
        return None

class ATM:
    def __init__(self):
        self.Balance = 5000
        self.Ac = None

    def deposit(self, amount):
        self.Balance += amount
        print(f"{amount} rupees deposited successfully.")
        print("Your current balance is:", self.Balance)
        self.save_transaction(credit=amount, debit=0)

    def withdraw(self, amount):
        if amount > self.Balance:
            print("Insufficient funds!")
        else:
            self.Balance -= amount
            print(f"{amount} rupees withdrawn successfully.")
        print("Your current balance is:", self.Balance)
        self.save_transaction(credit=0, debit=amount)

    def save_transaction(self, credit, debit):
        connection = establish_database_connection()
        if connection is None:
            return
        cursor = connection.cursor()
        sql = "INSERT INTO `atm` (`account_number`, `credit_amt`, `debit_amt`) VALUES (%s, %s, %s)"
        data = (self.Ac, credit, debit)
        cursor.execute(sql, data)
        connection.commit()
        cursor.close()
        connection.close()

    def receipt(self):
        print("Thanks for using SBI ATM")
        print("------------------------")
        print(f"Account No: {self.Ac}")
        print(f"Your current balance is: {self.Balance}")
        print("------------------------")
        print("Visit Again!")
        print("------------------------")
        x = datetime.datetime.now()
        print(x)
    def last(self):
        print(f"Account No:{self.Ac}")
        print("Thanks for using SBI ATM")
        print("Visit Again!")
        print("Have A Nice Day!.")
        x = datetime.datetime.now()
        print(x)

# Main Logic
Machine = ATM()

Ac = input("Enter the Account Number: ")
password = input("Enter the Password: ")

if Ac == "A122826":
    Machine.Ac = Ac
    if password == "1234":
        print("Logged in successfully!")

        while True:
            print("----------------------------------")
            print("     Welcome To SBI Net Banking    ")
            print("----------------------------------")
            print("1. Balance Enquiry")
            print("2. Withdraw Amount")
            print("3. Deposit Amount")
            print("4. Exit")
            option = int(input("Enter the option: "))

            if option == 1:
                print("Your Current Balance is:", Machine.Balance)
            elif option == 2:
                amount = float(input("Enter the amount to withdraw: "))
                Machine.withdraw(amount)
                Machine.last()
            elif option == 3:
                amount = float(input("Enter the amount to deposit: "))
                Machine.deposit(amount)
                Machine.last()
            elif option == 4:
                Machine.receipt()
                break
            else:
                print("Invalid option. Please try again.")
    else:
        print("Incorrect password.")
else:
    print("Incorrect Account number.")
