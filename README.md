import random
import string
import tkinter as tk
from tkinter import messagebox

# Function to generate a random password
def generate_password(length=12, use_lowercase=True, use_uppercase=True, use_digits=True, use_special_chars=True):
    characters = ''
    
    if use_lowercase:
        characters += string.ascii_lowercase
    if use_uppercase:
        characters += string.ascii_uppercase
    if use_digits:
        characters += string.digits
    if use_special_chars:
        characters += string.punctuation
    
    if not characters:
        return "No characters selected"
    
    password = ''.join(random.choice(characters) for _ in range(length))
    return password

# Function to check the strength of a password
def check_password_strength(password):
    length = len(password)
    complexity = 0
    
    if length >= 8:
        complexity += 1
    if any(char.islower() for char in password) and any(char.isupper() for char in password):
        complexity += 1
    if any(char.isdigit() for char in password):
        complexity += 1
    if any(char in string.punctuation for char in password):
        complexity += 1
    
    return complexity

# Function to display password strength tips
def show_strength_tips():
    tips = [
        "Use at least 8 characters",
        "Include a mix of uppercase and lowercase letters",
        "Include numbers and special characters",
        "Avoid common words or phrases",
        "Avoid using easily guessable information like your name or birthdate"
    ]
    
    messagebox.showinfo("Password Strength Tips", "\n".join(tips))

# Function to save passwords to a file
def save_password(password):
    with open("passwords.txt", "a") as file:
        file.write(password + "\n")

# Function to generate and display a password with strength information
def generate_and_display_password():
    length = int(length_entry.get())
    use_lowercase = lowercase_var.get() == 1
    use_uppercase = uppercase_var.get() == 1
    use_digits = digits_var.get() == 1
    use_special_chars = special_chars_var.get() == 1
    
    password = generate_password(length, use_lowercase, use_uppercase, use_digits, use_special_chars)
    strength = check_password_strength(password)
    
    password_label.config(text="Generated Password: " + password)
    strength_label.config(text="Password Strength: " + str(strength) + "/4")
    
    save_password(password)

# Create a GUI window
window = tk.Tk()
window.title("Password Generator")
window.geometry("400x300")

# Length label and entry
length_label = tk.Label(window, text="Enter the desired password length:")
length_label.pack()
length_entry = tk.Entry(window)
length_entry.pack()

# Character options
lowercase_var = tk.IntVar()
lowercase_checkbox = tk.Checkbutton(window, text="Include lowercase letters", variable=lowercase_var)
lowercase_checkbox.pack()

uppercase_var = tk.IntVar()
uppercase_checkbox = tk.Checkbutton(window, text="Include uppercase letters", variable=uppercase_var)
uppercase_checkbox.pack()

digits_var = tk.IntVar()
digits_checkbox = tk.Checkbutton(window, text="Include digits", variable=digits_var)
digits_checkbox.pack()

special_chars_var = tk.IntVar()
special_chars_checkbox = tk.Checkbutton(window, text="Include special characters", variable=special_chars_var)
special_chars_checkbox.pack()

# Generate password button
generate_button = tk.Button(window, text="Generate Password", command=generate_and_display_password)
generate_button.pack()

# Display generated password and strength
password_label = tk.Label(window, text="")
password_label.pack()
strength_label = tk.Label(window, text="")
strength_label.pack()

# Strength tips button
strength_tips_button = tk.Button(window, text="Password Strength Tips", command=show_strength_tips)
strength_tips_button.pack()

# Run the GUI
window.mainloop()
