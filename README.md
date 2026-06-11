# Advanced-Calculator 
import tkinter as tk
import math

# Global variables to track state
expression = ""
history_text = ""

# 1. Feature: Update expression on button press
def press(num):
    global expression
    expression = expression + str(num)
    equation.set(expression)

# 2. Feature: Dynamic Math Evaluation & History Tracking
def equalpress():
    try:
        global expression, history_text
        
        # Format display friendly power symbol '^' to Python's '**'
        calc_expression = expression.replace('^', '**')
        
        # Evaluate the math
        total = str(eval(calc_expression))
        
        # Update History display
        history_text = expression + " ="
        history_label.set(history_text)
        
        # Update Main display
        equation.set(total)
        expression = total  # Store answer for subsequent calculations
    except:
        equation.set(" Error ")
        expression = ""

# 3. Feature: Backspace (Delete last character)
def backspace():
    global expression
    expression = expression[:-1]
    equation.set(expression)

# 4. Feature: Square Root Operator
def square_root():
    try:
        global expression, history_text
        val = float(eval(expression.replace('^', '**'))) if expression else 0
        res = math.sqrt(val)
        
        history_label.set(f"√({expression}) =")
        equation.set(str(res))
        expression = str(res)
    except:
        equation.set(" Error ")
        expression = ""

# Clear screen
def clear():
    global expression, history_text
    expression = ""
    equation.set("")
    history_label.set("")

# --- GUI Window Setup ---
root = tk.Tk()
root.title("Advanced Visual Calculator")
root.geometry("340x480")
root.configure(bg="#1e272e")

equation = tk.StringVar()
history_label = tk.StringVar()

# Live History Display (Small text above main input)
history_screen = tk.Label(root, textvariable=history_label, font=("Arial", 11), fg="#a4b0be", bg="#1e272e", anchor="e", justify="right")
history_screen.pack(fill="x", padx=15, pady=(15, 0))

# Main Calculation Screen
display = tk.Entry(root, textvariable=equation, font=("Arial", 24, "bold"), bd=0, bg="#1e272e", fg="#ffffff", justify="right")
display.pack(fill="x", padx=15, pady=(0, 15), ipady=10)

# Grid Frame for buttons
button_frame = tk.Frame(root, bg="#1e272e")
button_frame.pack()

# Button configuration templates
btn_style = {'font': ('Arial', 14), 'fg': '#ffffff', 'bg': '#2f3542', 'activebackground': '#57606f', 'borderwidth': 0, 'height': 2, 'width': 5}
op_style = {**btn_style, 'bg': '#eccc68', 'fg': '#2f3542', 'activebackground': '#ffa502'} # Operator styling
fn_style = {**btn_style, 'bg': '#747d8c', 'activebackground': '#a4b0be'} # Function styling (C, Backspace)

# Button Layout Matrix (Text, Row, Column, Action Type)
layout = [
    ('C', 0, 0, 'fn'), ('⌫', 0, 1, 'fn'), ('^', 0, 2, 'op'), ('√', 0, 3, 'op'),
    ('7', 1, 0, 'num'), ('8', 1, 1, 'num'), ('9', 1, 2, 'num'), ('/', 1, 3, 'op'),
    ('4', 2, 0, 'num'), ('5', 2, 1, 'num'), ('6', 2, 2, 'num'), ('*', 2, 3, 'op'),
    ('1', 3, 0, 'num'), ('2', 3, 1, 'num'), ('3', 3, 2, 'num'), ('-', 3, 3, 'op'),
    ('0', 4, 0, 'num'), ('.', 4, 1, 'num'), ('=', 4, 2, 'eq'),  ('+', 4, 3, 'op'),
]

# Dynamically generate buttons onto the screen grid
for (text, r, c, t) in layout:
    if text == '=':
        btn = tk.Button(button_frame, text=text, command=equalpress, **{**btn_style, 'bg': '#ff4757', 'activebackground': '#ff6b81', 'width': 5})
    elif text == 'C':
        btn = tk.Button(button_frame, text=text, command=clear, **fn_style)
    elif text == '⌫':
        btn = tk.Button(button_frame, text=text, command=backspace, **fn_style)
    elif text == '√':
        btn = tk.Button(button_frame, text=text, command=square_root, **op_style)
    elif t == 'op':
        btn = tk.Button(button_frame, text=text, command=lambda x=text: press(x), **op_style)
    else:
        btn = tk.Button(button_frame, text=text, command=lambda x=text: press(x), **btn_style)
        
    btn.grid(row=r, column=c, padx=4, pady=4)

root.mainloop()
