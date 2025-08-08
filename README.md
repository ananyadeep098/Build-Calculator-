# Build-Calculator-
Create a command-line calculator to perform basic arithmetic operation. 
def add(x, y):
    return x + y

def subtract(x, y):
    return x - y

def multiply(x, y):
    return x * y

def divide(x, y):
    return x / y

while True:
    print("\nSelect operation:")
    print("1. Add")
    print("2. Subtract")
    print("3. Multiply")
    print("4. Divide")
    print("5. Exit")

    choice = input("Enter choice (1/2/3/4/5): ")

    if choice == '5':
        print("Exiting the calculator. Goodbye!")
        break

    try:
        num1 = float(input("Enter first number: "))
        num2 = float(input("Enter second number: "))

        if choice == '1':
            result = add(num1, num2)
            print(f"The result of {num1} + {num2} is {result}")
        elif choice == '2':
            result = subtract(num1, num2)
            print(f"The result of {num1} - {num2} is {result}")
        elif choice == '3':
            result = multiply(num1, num2)
            print(f"The result of {num1} * {num2} is {result}")
        elif choice == '4':
            try:
                result = divide(num1, num2)
                print(f"The result of {num1} / {num2} is {result}")
            except ZeroDivisionError:
                print("Error: Cannot divide by zero.")
        else:
            print("Invalid input. Please choose a valid option.")

    except ValueError:
        print("Error: Please enter valid numbers.")
