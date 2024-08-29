from tkinter import *
from tkinter import messagebox  # Import messagebox for error handling
from tkinter import ttk  # Import ttk for the Combobox
import random  # Import random for generating receipt numbers

# Global variables
main_window = None
entry_full_name = None
entry_item_hired = None
entry_quantity = None
entry_items = None  # Variable for the Dropbox
delete_item = None
hire_details = []
counters = {'total_entries': 0}

# Validate full name input for letters and spaces
def validate_full_name(*args):
    input_value = entry_full_name_var.get()
    if input_value.isalpha() or input_value == "":
        entry_full_name.config(bg='white')
    else:
        entry_full_name.config(bg='lightcoral')

# Quit subroutine
def quit():
    main_window.destroy()

# Print details of all the items
def print_item_details():
    # Clear existing labels before printing again
    clear_labels()
   
    # Create the column headings
    Label(main_window, font=("Arial", 10, "bold"), text="Row", bg="white").grid(column=0, row=7)
    Label(main_window, font=("Helvetica", 10, "bold"), text="Customer Full Name", bg="white").grid(column=1, row=7)
    Label(main_window, font=("Helvetica", 10, "bold"), text="Receipt Number", bg="white").grid(column=2, row=7)
    Label(main_window, font=("Helvetica", 10, "bold"), text="Item Hired", bg="white").grid(column=3, row=7)
    Label(main_window, font=("Helvetica", 10, "bold"), text="Quantity", bg="white").grid(column=4, row=7)
   
    # Add each item in the list into its own row
    for index, hire_detail in enumerate(hire_details):
        row_number = index + 8  # Start from row 8 onwards
        Label(main_window, text=index, font=("Helvetica", 10, "bold"), bg="#d3d3d3").grid(column=0, row=row_number)
        Label(main_window, text=hire_detail[0], font=("Helvetica", 10, "bold"), bg="#d3d3d3").grid(column=1, row=row_number)
        Label(main_window, text=hire_detail[1], font=("Helvetica", 10, "bold"), bg="#d3d3d3").grid(column=2, row=row_number)
        Label(main_window, text=hire_detail[2], font=("Helvetica", 10, "bold"), bg="#d3d3d3").grid(column=3, row=row_number)
        Label(main_window, text=hire_detail[3], font=("Helvetica", 10, "bold"), bg="#d3d3d3").grid(column=4, row=row_number)

# Clear labels function
def clear_labels():
    # Clear previous labels in the rows starting from row 8
    for widget in main_window.grid_slaves():
        if int(widget.grid_info()["row"]) >= 8:
            widget.grid_forget()

# Add the next hired item to the list
def append_item():
    global hire_details, counters
    full_name = entry_full_name_var.get()
    quantity_text = entry_quantity.get()
    
    # Check if full name is not blank and valid
    if len(full_name) == 0:
        messagebox.showerror("Error", "Customer Full Name cannot be blank.")
        return
    if not full_name.isalpha():
        messagebox.showerror("Error", "Customer Full Name can only contain letters and spaces.")
        return

    # Try to convert quantity to an integer and check if it's within the range
    try:
        quantity = int(quantity_text)
        if 1 <= quantity <= 500:
            # Get the selected item from the Dropbox
            item_hired = entry_items.get()
            # Generate a random receipt number
            receipt_number = random.randint(1000, 9999)
            # Append each item to its own area of the list
            hire_details.append([full_name, str(receipt_number), item_hired, str(quantity)])
            # Clear the boxes
            entry_full_name_var.set("")
            entry_quantity.delete(0, 'end')
            counters['total_entries'] += 1
        else:
            messagebox.showerror("Error", "Quantity must be between 1 and 500.")
    except ValueError:
        messagebox.showerror("Error", "Quantity must be a valid integer.")

# Delete a row from the list
def delete_row():
    global hire_details, counters
    # Find which row is to be deleted and delete it
    try:
        index_to_delete = int(delete_item.get())
        if 0 <= index_to_delete < counters['total_entries']:
            del hire_details[index_to_delete]
            counters['total_entries'] -= 1
            clear_labels()
            print_item_details()
        else:
            messagebox.showerror("Error", "Row number out of range.")
    except ValueError:
        messagebox.showerror("Error", "Row number must be a valid integer.")
    delete_item.delete(0, 'end')

# Create the buttons and labels
def setup_buttons():
    global main_window, entry_full_name_var, entry_full_name, entry_item_hired, entry_quantity, delete_item, entry_items
   
    main_window.configure(bg='#906f7c')  # Set background color to purple
   
    Label(main_window, bg='white', font=("Arial", 20, "bold"), text="Julies Party Hire", relief='ridge', borderwidth=4).grid(column=1, row=0, padx=10, pady=10)
    Label(main_window, bg='white', font=("Arial", 10, "bold"), text="Item Hired", relief='ridge', borderwidth=4).grid(column=0, row=3, padx=10, pady=10)
    Label(main_window, bg='white', font=("Arial", 10, "bold"), text="Quantity", relief='ridge', borderwidth=4).grid(column=0, row=5, padx=10, pady=10)
    Label(main_window, bg='white', font=("Arial", 10, "bold"), text="Row #", relief='ridge', borderwidth=4).grid(column=1, row=5, padx=10, pady=10)
    Label(main_window, bg='white', font=("Arial", 10, "bold"), text="Customer Full Name", relief='ridge', borderwidth=4).grid(column=0, row=1, padx=10, pady=10)

    Button(main_window, bg='grey', font=("Arial", 13, "bold"), text="Quit", command=quit, relief='sunken', borderwidth=4).grid(column=5, row=1, padx=10, pady=10)
    Button(main_window, bg='white', font=("Arial", 10, "bold"), text="Append Details", command=append_item, relief='ridge', borderwidth=4).grid(column=1, row=1, padx=10, pady=10)
    Button(main_window, bg='white', font=("Arial", 10, "bold"), text="Print Details", command=print_item_details, relief='ridge', borderwidth=4).grid(column=1, row=2, padx=10, pady=10)
    Button(main_window, bg='white', font=("Arial", 10, "bold"), text="Delete", command=delete_row, relief='sunken', borderwidth=4).grid(column=2, row=6, padx=10, pady=10)
   
    entry_full_name_var = StringVar()
    entry_full_name_var.trace_add("write", validate_full_name)
    
    entry_full_name = Entry(main_window, bg='white', textvariable=entry_full_name_var)
    entry_full_name.grid(column=0, row=2, padx=10, pady=10)

    entry_quantity = Entry(main_window, bg='white')
    entry_quantity.grid(column=0, row=6, padx=10, pady=10)
   
    delete_item = Entry(main_window, bg='white')
    delete_item.grid(column=1, row=6, padx=10, pady=10)
    
    # Adding the Combobox for item selection
    n = StringVar()
    entry_items = ttk.Combobox(main_window, width=12, textvariable=n)
    entry_items['values'] = ('Party Hat', 'Balloon', 'Disco Ball', 'Party Popper', 'Piñata')  # Update with actual item names
    entry_items.grid(column=0, row=4)
    entry_items.current()  # Set the default selection to the first item

# Start the program running
def main():
    global main_window
    main_window = Tk()
    main_window.configure(bg='#906f7c')  # Set background color to purple
   
    setup_buttons()
   
    main_window.mainloop()  # Start the Tkinter event loop

# Entry point of the program
if __name__ == "__main__":
    main()
