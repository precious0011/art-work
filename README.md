import tkinter as tk
from tkinter import messagebox
import sqlite3

# ---------------- DATABASE ----------------
conn = sqlite3.connect("glh.db")
cursor = conn.cursor()

cursor.execute("""
CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username TEXT,
    email TEXT,
    password TEXT,
    address TEXT,
    county TEXT
)
""")
conn.commit()

# ---------------- MAIN WINDOW ----------------
root = tk.Tk()
root.title("Greenfield Local Hub")
root.geometry("600x500")
root.configure(bg="#e8f5e9")

# ---------------- HEADER ----------------
header = tk.Frame(root, bg="#2e7d32", height=80)
header.pack(fill="x")

tk.Label(header, text="Greenfield Local Hub",
         font=("Arial", 18, "bold"),
         fg="white", bg="#2e7d32").pack(pady=20)

# ---------------- CARD ----------------
card = tk.Frame(root, bg="white", bd=2, relief="raised")
card.place(relx=0.5, rely=0.5, anchor="center", width=350, height=320)

tk.Label(card, text="Welcome", font=("Arial", 14, "bold"), bg="white").pack(pady=10)
tk.Label(card, text="Select your role", bg="white").pack(pady=5)

# ---------------- FUNCTIONS ----------------

def open_customer():
    open_login()

def open_producer():
    messagebox.showinfo("Info", "Producer dashboard coming soon")

# -------- LOGIN PAGE --------
def open_login():
    login_win = tk.Toplevel(root)
    login_win.title("Customer Login")
    login_win.geometry("350x300")

    tk.Label(login_win, text="Customer Login", font=("Arial", 14)).pack(pady=10)

    tk.Label(login_win, text="Email").pack()
    email = tk.Entry(login_win)
    email.pack()

    tk.Label(login_win, text="Password").pack()
    password = tk.Entry(login_win, show="*")
    password.pack()

    def login():
        cursor.execute("SELECT * FROM users WHERE email=? AND password=?",
                       (email.get(), password.get()))
        user = cursor.fetchone()

        if user:
            messagebox.showinfo("Success", "Login successful")
            login_win.destroy()
            open_dashboard(user)
        else:
            messagebox.showerror("Error", "Invalid details")

    tk.Button(login_win, text="Login", bg="#4CAF50", fg="white",
              command=login).pack(pady=10)

    tk.Button(login_win,
              text="Don't have an account?",
              fg="blue",
              command=lambda: open_register(login_win)).pack()

# -------- REGISTER PAGE --------
def open_register(parent):
    parent.destroy()

    reg_win = tk.Toplevel(root)
    reg_win.title("Register")
    reg_win.geometry("350x450")

    tk.Label(reg_win, text="Create Account", font=("Arial", 14)).pack(pady=10)

    tk.Label(reg_win, text="Username").pack()
    username = tk.Entry(reg_win)
    username.pack()

    tk.Label(reg_win, text="Email Address").pack()
    email = tk.Entry(reg_win)
    email.pack()

    tk.Label(reg_win, text="Password").pack()
    password = tk.Entry(reg_win, show="*")
    password.pack()

    tk.Label(reg_win, text="Home Address").pack()
    address = tk.Entry(reg_win)
    address.pack()

    tk.Label(reg_win, text="County").pack()
    county = tk.Entry(reg_win)
    county.pack()

    def register():
        if (username.get() == "" or email.get() == "" or
            password.get() == "" or address.get() == "" or county.get() == ""):
            messagebox.showerror("Error", "Fill all fields")
            return

        cursor.execute("""
        INSERT INTO users (username, email, password, address, county)
        VALUES (?, ?, ?, ?, ?)
        """, (username.get(), email.get(), password.get(),
              address.get(), county.get()))
        conn.commit()

        messagebox.showinfo("Success", "Account created")
        reg_win.destroy()
        open_dashboard()

    tk.Button(reg_win, text="Register", bg="#4CAF50", fg="white",
              command=register).pack(pady=15)

# -------- DASHBOARD (SHOP UI) --------
def open_dashboard(user=None):
    dash = tk.Toplevel(root)
    dash.title("Customer Dashboard")
    dash.geometry("600x500")
    dash.configure(bg="white")

    # SEARCH BAR
    search_frame = tk.Frame(dash, bg="#eeeeee")
    search_frame.pack(fill="x")

    search_entry = tk.Entry(search_frame, width=40)
    search_entry.pack(side="left", padx=10, pady=10)

    tk.Button(search_frame, text="Search").pack(side="left")

    # CATEGORY BAR
    category_frame = tk.Frame(dash, bg="white")
    category_frame.pack(fill="x")

    for cat in ["All", "Deals", "Popular", "Fresh"]:
        tk.Button(category_frame, text=cat, bd=0).pack(side="left", padx=10)

    # PRODUCT GRID
    product_frame = tk.Frame(dash, bg="white")
    product_frame.pack(pady=10)

    # LOAD IMAGES
    milk_img = tk.PhotoImage(file="images/milk.png")
    bread_img = tk.PhotoImage(file="images/bread.png")
    eggs_img = tk.PhotoImage(file="images/eggs.png")
    veg_img = tk.PhotoImage(file="images/veg.png")

    products = [
        ("Milk", "£1.50", milk_img),
        ("Bread", "£1.20", bread_img),
        ("Eggs", "£2.50", eggs_img),
        ("Vegetables", "£3.00", veg_img),
    ]

    row = 0
    col = 0

    for name, price, img in products:
        frame = tk.Frame(product_frame, bd=1, relief="solid", bg="white")
        frame.grid(row=row, column=col, padx=10, pady=10)

        img_label = tk.Label(frame, image=img, bg="white")
        img_label.image = img
        img_label.pack()

        tk.Label(frame, text=name, bg="white").pack()
        tk.Label(frame, text=price, fg="green", bg="white").pack()

        tk.Button(frame, text="Add to Cart").pack(pady=5)

        col += 1
        if col == 2:
            col = 0
            row += 1

# ---------------- BUTTONS ----------------
tk.Button(card, text="Customer", width=20,
          bg="#4CAF50", fg="white",
          command=open_customer).pack(pady=10)

tk.Button(card, text="Producer", width=20,
          bg="#1976D2", fg="white",
          command=open_producer).pack()

# ---------------- FOOTER ----------------
tk.Label(root,
         text="Supporting Local Farmers & Producers",
         bg="#e8f5e9").pack(side="bottom", pady=10)

# ---------------- RUN ----------------
root.mainloop()
