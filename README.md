# 91896---Develop-a-Computer-problem
The project will look at developing a computer program that solves a certain problem.
from tkinter import *

# Initialize hire_details list
hire_details = []

# quit subroutine
def quit_program():
    main_window.destroy()

# print details of all the hires
def print_hire_details():
    # Clear previous entries
    for widget in main_window.grid_slaves():
        if int(widget.grid_info()["row"]) >= 8:
            widget.grid_forget()

    # Headers
    Label(main_window, font='bold', text="Row").grid(column=0, row=7)
    Label(main_window, font='bold', text="Customer Full Name").grid(column=1, row=7)
    Label(main_window, font='bold', text="Receipt Number").grid(column=2, row=7)
    Label(main_window, font='bold', text="Item Hired").grid(column=3, row=7)
    Label(main_window, font='bold', text="Quantity").grid(column=4, row=7)

    # Data rows
    for row, hire_detail in enumerate(hire_details):
        Label(main_window, text=row).grid(column=0, row=row+8)
        Label(main_window, text=hire_detail[0]).grid(column=1, row=row+8)
        Label(main_window, text=hire_detail[1]).grid(column=2, row=row+8)
        Label(main_window, text=hire_detail[2]).grid(column=3, row=row+8)
        Label(main_window, text=hire_detail[3]).grid(column=4, row=row+8)

# add the next hire details to the list
def append_hire_details():
    customer_name = entry_customer_name.get()
    receipt_number = entry_receipt_number.get()
    item_hired = entry_item_hired.get()
    quantity = entry_quantity.get()

    if customer_name and receipt_number and item_hired and quantity:
        hire_details.append([customer_name, receipt_number, item_hired, quantity])

        # Clear entry fields after appending
        entry_customer_name.delete(0, 'end')
        entry_receipt_number.delete(0, 'end')
        entry_item_hired.delete(0, 'end')
        entry_quantity.delete(0, 'end')

        # Update display
        print_hire_details()

# delete a row from the list
def delete_row():
    try:
        index = int(delete_item.get())
        del hire_details[index]

        # Clear entry field after deletion
        delete_item.delete(0, 'end')

        # Update display
        print_hire_details()
    except ValueError:
        pass  # Handle if the input is not a valid integer

# create the buttons and labels
def setup_buttons():
    Button(main_window, text="Quit", command=quit_program).grid(column=7, row=6)
    Button(main_window, text="Append Hire Details", command=append_hire_details).grid(column=0, row=6)
    Button(main_window, text="Print Details", command=print_hire_details).grid(column=1, row=6)
    Label(main_window, text="Customer Full Name").grid(column=0, row=1)
    Label(main_window, text="Receipt Number").grid(column=0, row=2)
    Label(main_window, text="Item Hired").grid(column=0, row=3)
    Label(main_window, text="Quantity").grid(column=0, row=4)

    Label(main_window, text="Enter row # to delete:").grid(column=0, row=5)
    Button(main_window, text="Delete", command=delete_row).grid(column=3, row=6)

# start the program running
def main():
    global main_window, entry_customer_name, entry_receipt_number, entry_item_hired, entry_quantity, delete_item

    main_window = Tk()
    main_window.title("Hire Details Management")

    entry_customer_name = Entry(main_window)
    entry_customer_name.grid(column=1, row=1)

    entry_receipt_number = Entry(main_window)
    entry_receipt_number.grid(column=1, row=2)

    entry_item_hired = Entry(main_window)
    entry_item_hired.grid(column=1, row=3)

    entry_quantity = Entry(main_window)
    entry_quantity.grid(column=1, row=4)

    delete_item = Entry(main_window)
    delete_item.grid(column=1, row=5)

    setup_buttons()

    main_window.mainloop()

if __name__ == "__main__":
    main()
