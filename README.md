# Drijou-
المحاسبة والمخازن والفواتير
import sqlite3
‏from datetime import datetime

‏# Connect to SQLite database
‏conn = sqlite3.connect('invoice_system.db')
‏cursor = conn.cursor()

‏# Create tables for customers and products
‏cursor.execute('''
‏CREATE TABLE IF NOT EXISTS customers (
‏    id INTEGER PRIMARY KEY,
‏    name TEXT NOT NULL,
‏    company_name TEXT,
‏    address TEXT,
‏    phone TEXT
)
''')

‏cursor.execute('''
‏CREATE TABLE IF NOT EXISTS products (
‏    id INTEGER PRIMARY KEY,
‏    name TEXT NOT NULL,
‏    price REAL NOT NULL,
‏    tax REAL NOT NULL
)
''')

‏# Function to add a new invoice
‏def create_invoice(customer_id, items):
‏    date_of_sale = datetime.now().strftime('%Y-%m-%d')
‏    invoice_number = str(cursor.execute('SELECT COUNT(*) FROM invoices').fetchone()[0] + 1).zfill(7)
‏    total_net = sum(item['price'] * item['quantity'] for item in items)
‏    total_tax_7 = sum(item['price'] * item['quantity'] * 0.07 for item in items if item['tax'] == 7)
‏    total_tax_19 = sum(item['price'] * item['quantity'] * 0.19 for item in items if item['tax'] == 19)
‏    total_gross = total_net + total_tax_7 + total_tax_19
    
‏    # Logic to create invoice using above information can be added here
    
‏    print(f'Invoice Number: {invoice_number}')
‏    print(f'Date: {date_of_sale}')
‏    print(f'Total Net: {total_net}')
‏    print(f'Total Tax 7%: {total_tax_7}')
‏    print(f'Total Tax 19%: {total_tax_19}')
‏    print(f'Total Gross: {total_gross}')

‏# Test data and execution
‏customer_id = 1
‏items = [
‏    {'name': 'Apple', 'quantity': 10, 'price': 1.5, 'tax': 7},
‏    {'name': 'Banana', 'quantity': 5, 'price': 0.8, 'tax': 19}
]

‏create_invoice(customer_id, items)

‏# Close the database connection
‏conn.close()
