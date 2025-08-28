import tkinter as tk
from tkinter import messagebox

class Calculator:
    def __init__(self, root):
        self.root = root
        self.root.title("Python Calculator")
        self.root.geometry("350x500")
        self.root.resizable(False, False)
        
        self.current_input = tk.StringVar()
        self.current_input.set("0")
        
        self.expression_var = tk.StringVar()   
        self.expression = ""
        self.operation_in_progress = False
        
        self.create_display()
        self.create_buttons()
        
    def create_display(self):
        display_frame = tk.Frame(self.root, height=120)
        display_frame.pack(expand=True, fill="both", padx=10, pady=10)
        
        expression_label = tk.Label(
            display_frame,
            textvariable=self.expression_var,
            anchor="e",
            bg="#f0f0f0",
            fg="gray",
            font=("Arial", 16),
            padx=10
        )
        expression_label.pack(expand=True, fill="both")
        
        display = tk.Label(
            display_frame, 
            textvariable=self.current_input, 
            anchor="e", 
            bg="#f0f0f0", 
            fg="#000", 
            font=("Arial", 28, "bold"),
            padx=10
        )
        display.pack(expand=True, fill="both")
        
    def create_buttons(self):
        button_frame = tk.Frame(self.root)
        button_frame.pack(expand=True, fill="both", padx=10, pady=10)
        
        buttons = [
            ['C', '⌫', '%', '/'],
            ['7', '8', '9', '*'],
            ['4', '5', '6', '-'],
            ['1', '2', '3', '+'],
            ['00', '0', '.', '=']
        ]
        
        for i, row in enumerate(buttons):
            button_frame.rowconfigure(i, weight=1)
            for j, btn_text in enumerate(row):
                button_frame.columnconfigure(j, weight=1)
                
                btn = tk.Button(
                    button_frame,
                    text=btn_text,
                    font=("Arial", 18),
                    command=lambda x=btn_text: self.on_button_click(x)
                )
                
                if btn_text in ['C', '⌫']:
                    btn.config(bg="#ff6b6b", fg="white")
                elif btn_text in ['/', '*', '-', '+', '=', '%']:
                    btn.config(bg="#4ecdc4", fg="white")
                
                btn.grid(row=i, column=j, sticky="nsew", padx=2, pady=2)
    
    def on_button_click(self, button_text):
        try:
            if button_text == 'C':
                self.clear()
            elif button_text == '⌫':
                self.backspace()
            elif button_text == '=':
                self.calculate()
            elif button_text == '%':
                self.percentage()
            elif button_text in ['/', '*', '-', '+']:
                self.add_operator(button_text)
            else:
                self.add_digit(button_text)
        except Exception as e:
            messagebox.showerror("Error", str(e))
            self.clear()
    
    def clear(self):
        self.current_input.set("0")
        self.expression = ""
        self.expression_var.set("")
        self.operation_in_progress = False
    
    def backspace(self):
        current = self.current_input.get()
        if len(current) == 1 or (len(current) == 2 and current.startswith('-')):
            self.current_input.set("0")
        else:
            self.current_input.set(current[:-1])
    
    def add_digit(self, digit):
        current = self.current_input.get()
        
        if self.operation_in_progress:
            current = "0"
            self.operation_in_progress = False
            
        if current == "0" and digit != '.':
            current = digit
        else:
            if digit == '.' and '.' in current:
                return
            current += digit
            
        self.current_input.set(current)
    
    def add_operator(self, operator):
        current = self.current_input.get()
        
        if self.operation_in_progress:
            self.expression = current + operator
            self.expression_var.set(self.expression)
            self.operation_in_progress = False
            return
            
        self.expression += current + operator
        self.expression_var.set(self.expression)   
        self.operation_in_progress = True
    
    def calculate(self):
        self.expression += self.current_input.get()
        
        try:
            result = eval(self.expression)
            self.current_input.set(str(result))
            self.expression_var.set(self.expression + " =")  
            self.expression = ""
            self.operation_in_progress = True
        except ZeroDivisionError:
            messagebox.showerror("Error", "Cannot divide by zero!")
            self.clear()
        except Exception:
            messagebox.showerror("Error", "Invalid calculation!")
            self.clear()
    
    def percentage(self):
        try:
            current = float(self.current_input.get())
            result = current / 100
            self.current_input.set(str(result))
        except:
            messagebox.showerror("Error", "Invalid value for percentage!")
            self.clear()

if __name__ == "__main__":
    root = tk.Tk()
    calculator = Calculator(root)
    root.mainloop()
