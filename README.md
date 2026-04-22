def open_producer_dashboard():
    dash = tk.Toplevel(root)
    dash.title("Producer Dashboard")
    dash.geometry("1000x600")
    dash.configure(bg="#f0f0f0")

    # -------- SIDEBAR --------
    sidebar = tk.Frame(dash, bg="#1e1e2f", width=200)
    sidebar.pack(side="left", fill="y")

    tk.Label(sidebar, text="GLH Admin",
             fg="white", bg="#1e1e2f",
             font=("Arial", 14, "bold")).pack(pady=20)

    # -------- MAIN CONTENT --------
    content = tk.Frame(dash, bg="#f5f5f5")
    content.pack(side="right", expand=True, fill="both")

    def clear():
        for w in content.winfo_children():
            w.destroy()

    # -------- HOME --------
    def home():
        clear()
        tk.Label(content, text="Welcome, Producer",
                 font=("Arial", 18, "bold"),
                 bg="#f5f5f5").pack(pady=30)

        tk.Label(content, text="Use the menu to manage products, stock and orders",
                 bg="#f5f5f5").pack()

    # -------- VIEW PRODUCTS --------
    def view_products():
        clear()

        tk.Label(content, text="Products",
                 font=("Arial", 16, "bold"),
                 bg="#f5f5f5").pack(pady=10)

        cursor.execute("SELECT name, price, stock FROM products")

        for p in cursor.fetchall():
            box = tk.Frame(content, bg="white", bd=2, relief="groove", padx=10, pady=10)
            box.pack(fill="x", padx=20, pady=8)

            tk.Label(box, text=p[0],
                     font=("Arial", 12, "bold"),
                     bg="white").pack(side="left")

            tk.Label(box, text=f"£{p[1]}",
                     fg="green", bg="white").pack(side="left", padx=20)

            stock_color = "red" if p[2] < 5 else "green"
            tk.Label(box, text=f"Stock: {p[2]}",
                     fg=stock_color,
                     bg="white").pack(side="right")

    # -------- UPDATE STOCK --------
    def update_stock():
        clear()

        tk.Label(content, text="Update Stock",
                 font=("Arial", 16, "bold"),
                 bg="#f5f5f5").pack(pady=10)

        tk.Label(content, text="Product Name", bg="#f5f5f5").pack()
        name = tk.Entry(content)
        name.pack()

        tk.Label(content, text="New Stock", bg="#f5f5f5").pack()
        stock = tk.Entry(content)
        stock.pack()

        def update():
            try:
                value = int(stock.get())
                cursor.execute("UPDATE products SET stock=? WHERE name=?",
                               (value, name.get()))
                conn.commit()
                tk.messagebox.showinfo("Success", "Stock updated")
            except:
                tk.messagebox.showerror("Error", "Enter valid number")

        tk.Button(content, text="Update",
                  bg="#1976D2", fg="white",
                  width=15, command=update).pack(pady=10)

    # -------- VIEW ORDERS --------
    def view_orders():
        clear()

        tk.Label(content, text="Orders",
                 font=("Arial", 16, "bold"),
                 bg="#f5f5f5").pack(pady=10)

        cursor.execute("SELECT user_email, items, total FROM orders")

        for o in cursor.fetchall():
            box = tk.Frame(content, bg="white", bd=2, relief="groove", padx=10, pady=10)
            box.pack(fill="x", padx=20, pady=8)

            tk.Label(box, text=o[0],
                     font=("Arial", 10, "bold"),
                     bg="white").pack(anchor="w")

            tk.Label(box, text=o[1],
                     bg="white").pack(anchor="w")

            tk.Label(box, text=f"Total: £{o[2]}",
                     fg="green",
                     bg="white").pack(anchor="e")

    # -------- SIDEBAR BUTTONS --------
    def nav_button(text, command):
        return tk.Button(sidebar, text=text,
                         bg="#2e2e3f", fg="white",
                         relief="flat",
                         width=20, height=2,
                         command=command)

    nav_button("🏠 Home", home).pack(pady=5)
    nav_button("📦 Products", view_products).pack(pady=5)
    nav_button("📊 Update Stock", update_stock).pack(pady=5)
    nav_button("🧾 Orders", view_orders).pack(pady=5)

    # default screen
    home()


def open_producer_dashboard():
    dash = tk.Toplevel(root)
    dash.title("Producer Dashboard")
    dash.geometry("900x600")
    dash.configure(bg="#f5f5f5")

    # -------- HEADER --------
    header = tk.Frame(dash, bg="#2e7d32", height=60)
    header.pack(fill="x")

    tk.Label(header, text="Producer Dashboard",
             fg="white", bg="#2e7d32",
             font=("Arial", 16, "bold")).pack(pady=15)

    # -------- NAV BAR --------
    nav = tk.Frame(dash, bg="#eeeeee")
    nav.pack(fill="x")

    content = tk.Frame(dash, bg="#f5f5f5")
    content.pack(fill="both", expand=True, pady=10)

    # CLEAR CONTENT
    def clear():
        for w in content.winfo_children():
            w.destroy()

    # -------- VIEW PRODUCTS --------
    def view_products():
        clear()

        cursor.execute("SELECT name, price, stock FROM products")

        for p in cursor.fetchall():
            box = tk.Frame(content, bg="white", bd=2, relief="groove", padx=10, pady=10)
            box.pack(fill="x", padx=20, pady=8)

            tk.Label(box, text=p[0],
                     font=("Arial", 12, "bold"),
                     bg="white").pack(side="left")

            tk.Label(box, text=f"£{p[1]}",
                     fg="green", bg="white").pack(side="left", padx=20)

            stock_color = "red" if p[2] < 5 else "green"
            tk.Label(box, text=f"Stock: {p[2]}",
                     fg=stock_color,
                     bg="white").pack(side="right")

    # -------- UPDATE STOCK --------
    def update_stock():
        clear()

        tk.Label(content, text="Update Stock",
                 font=("Arial", 14, "bold"),
                 bg="#f5f5f5").pack(pady=10)

        tk.Label(content, text="Product Name", bg="#f5f5f5").pack()
        name = tk.Entry(content)
        name.pack()

        tk.Label(content, text="New Stock", bg="#f5f5f5").pack()
        stock = tk.Entry(content)
        stock.pack()

        def update():
            try:
                value = int(stock.get())
                cursor.execute("UPDATE products SET stock=? WHERE name=?",
                               (value, name.get()))
                conn.commit()
                messagebox.showinfo("Success", "Stock updated")
            except:
                messagebox.showerror("Error", "Enter valid number")

        tk.Button(content, text="Update",
                  bg="#1976D2", fg="white",
                  width=15, command=update).pack(pady=10)

    # -------- VIEW ORDERS --------
    def view_orders():
        clear()

        cursor.execute("SELECT user_email, items, total FROM orders")

        for o in cursor.fetchall():
            box = tk.Frame(content, bg="white", bd=2, relief="groove", padx=10, pady=10)
            box.pack(fill="x", padx=20, pady=8)

            tk.Label(box, text=o[0],
                     font=("Arial", 10, "bold"),
                     bg="white").pack(anchor="w")

            tk.Label(box, text=o[1],
                     bg="white").pack(anchor="w")

            tk.Label(box, text=f"Total: £{o[2]}",
                     fg="green",
                     bg="white").pack(anchor="e")

    # -------- BUTTONS --------
    tk.Button(nav, text="View Products",
              bg="#4CAF50", fg="white",
              width=20, command=view_products).pack(side="left", padx=10, pady=5)

    tk.Button(nav, text="Update Stock",
              bg="#1976D2", fg="white",
              width=20, command=update_stock).pack(side="left", padx=10, pady=5)

    tk.Button(nav, text="View Orders",
              bg="#FF9800", fg="white",
              width=20, command=view_orders).pack(side="left", padx=10, pady=5)

    # default view
    view_products()





    
    
