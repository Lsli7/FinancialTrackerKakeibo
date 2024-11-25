import tkinter as tk
from tkinter import ttk, messagebox
import csv
import os
from datetime import datetime
import matplotlib.pyplot as plt
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg


class FinancialApp:
    def __init__(self, root):
        self.root = root
        self.root.title("KAKEIBO Financial Tracker")
        self.transactions_file = 'transactions.csv'
        self.goals_file = 'goals.csv'
        self.create_files()
        self.transactions = self.load_data(self.transactions_file)
        self.goals = self.load_goals(self.goals_file)

        self.frame = tk.Frame(root)
        self.frame.pack(padx=20, pady=20)

        self.create_widgets()

    def create_files(self):
        '''Create the required CSV files if they are not already present'''
        if not os.path.exists(self.transactions_file):
            with open(self.transactions_file, 'w', newline='') as file:
                writer = csv.writer(file)
                writer.writerow(['Transaction Type', 'Amount', 'Category', 'Description', 'Date'])

        if not os.path.exists(self.goals_file):
            with open(self.goals_file, 'w', newline='') as file:
                writer = csv.writer(file)
                writer.writerow(['Goal Name', 'Target Amount', 'Target Date', 'Amount Saved'])

    def load_data(self, file_name):
        data = []
        if os.path.exists(file_name):
            with open(file_name, newline='') as file:
                reader = csv.reader(file)
                next(reader)  # Skip headers
                for row in reader:
                    data.append(row)
        return data

    def load_goals(self, file_name):
        goals = []
        if os.path.exists(file_name):
            with open(file_name, newline='') as file:
                reader = csv.reader(file)
                next(reader)  # Skip headers
                for row in reader:
                    goal_name, target_amount, target_date, amount_saved = row
                    goals.append([goal_name, float(target_amount), target_date, float(amount_saved)])
        return goals

    def create_widgets(self):

        self.dashboard_frame = tk.LabelFrame(self.frame, text="Financial Summary", padx=20, pady=20)
        self.dashboard_frame.grid(row=0, column=0, padx=10, pady=10)

        # Labels for Total Income, Expenses, Savings, Remaining Balance
        self.total_income_label = tk.Label(self.dashboard_frame, text="Total Income: $0", font=("Arial", 14))
        self.total_income_label.grid(row=0, column=0, padx=10, pady=10)

        self.total_expenses_label = tk.Label(self.dashboard_frame, text="Total Expenses: $0", font=("Arial", 14))
        self.total_expenses_label.grid(row=1, column=0, padx=10, pady=10)

        self.savings_label = tk.Label(self.dashboard_frame, text="Savings for Goals: $0", font=("Arial", 14))
        self.savings_label.grid(row=2, column=0, padx=10, pady=10)

        self.remaining_balance_label = tk.Label(self.dashboard_frame, text="Remaining Balance: $0", font=("Arial", 14))
        self.remaining_balance_label.grid(row=3, column=0, padx=10, pady=10)

        self.instruction_label = tk.Label(self.frame, text="Select an action to get started:",
                                          font=("Arial", 12, "italic"))
        self.instruction_label.grid(row=1, column=0, pady=10)

        # Buttons to Add Transaction, View Report, and Manage Goals
        self.add_transaction_button = tk.Button(self.frame, text="Add Transaction", command=self.add_transaction,
                                                width=20)
        self.add_transaction_button.grid(row=2, column=0, padx=10, pady=5)

        self.view_report_button = tk.Button(self.frame, text="Monthly Reflection", command=self.view_report, width=20)
        self.view_report_button.grid(row=3, column=0, padx=10, pady=5)

        self.manage_goals_button = tk.Button(self.frame, text="Manage Goals", command=self.manage_goals, width=20)
        self.manage_goals_button.grid(row=4, column=0, padx=10, pady=5)

        # Update the dashboard to show current financial summary
        self.update_dashboard()

    def update_dashboard(self):
        """ Update with totals and balances """
        total_income = sum(float(t[1]) for t in self.transactions if t[0] == 'Income')
        total_expenses = sum(float(t[1]) for t in self.transactions if t[0] == 'Expense')
        remaining_balance = total_income - total_expenses

        # Update Savings for Goals
        total_savings_for_goals = sum(float(goal[3]) for goal in self.goals)

        self.total_income_label.config(text="Total Income: $" + str(total_income))
        self.total_expenses_label.config(text="Total Expenses: $" + str(total_expenses))
        self.savings_label.config(text="Savings for Goals: $" + str(total_savings_for_goals))
        self.remaining_balance_label.config(text="Remaining Balance: $" + str(remaining_balance))

    def add_transaction(self):

        def save_transaction():
            try:
                transaction_type = transaction_type_var.get()
                amount = float(amount_entry.get())
                category = category_var.get()
                description = description_entry.get()
                date = date_entry.get()

                if not (transaction_type and amount and category and date):
                    raise ValueError("Please fill in all fields.")

                try:
                    datetime.strptime(date, "%Y-%m-%d")
                except ValueError:
                    raise ValueError("Invalid date format. Use YYYY-MM-DD.")

                transaction = [transaction_type, str(amount), category, description, date]
                self.transactions.append(transaction)

                # Save transaction to CSV file
                with open(self.transactions_file, 'a', newline='') as file:
                    writer = csv.writer(file)
                    writer.writerow(transaction)

                # Update dashboard
                self.update_dashboard()

                top.destroy()

            except ValueError as e:
                print("Input Error:", e)  # Using print instead of messagebox.showerror()

        '''Open a new dialog window for entering a transaction'''
        top = tk.Toplevel(self.root)
        top.title("KAKEIBO ADD Transaction")

        '''Get the position of the main window'''
        main_window_x = self.root.winfo_rootx()
        main_window_y = self.root.winfo_rooty()

        # Get the size of the main window
        main_window_width = self.root.winfo_width()
        main_window_height = self.root.winfo_height()

        # Center the new window using str.format() instead of f-string
        top.geometry("400x400+{0}+{1}".format(main_window_x + main_window_width // 2 - 200,
                                              main_window_y + main_window_height // 2 - 200))

        # Frame for layout
        frame = tk.Frame(top, padx=20, pady=20)
        frame.pack(padx=10, pady=10)

        # Title Label
        title_label = tk.Label(frame, text="Add a Transaction", font=("Arial", 16, "bold"))
        title_label.grid(row=0, column=0, columnspan=2, pady=10)

        # Transaction Type
        tk.Label(frame, text="Transaction Type:", font=("Arial", 12)).grid(row=1, column=0, sticky='e', pady=5)
        transaction_type_var = tk.StringVar(value='Income')
        ttk.Combobox(frame, textvariable=transaction_type_var, values=["Income", "Expense"], state="readonly",
                     font=("Arial", 12)).grid(row=1, column=1, pady=5)

        # Amount
        tk.Label(frame, text="Amount ($):", font=("Arial", 12)).grid(row=2, column=0, sticky='e', pady=5)
        amount_entry = tk.Entry(frame, font=("Arial", 12))
        amount_entry.grid(row=2, column=1, pady=5)

        # Category
        tk.Label(frame, text="Category:", font=("Arial", 12)).grid(row=3, column=0, sticky='e', pady=5)
        category_var = tk.StringVar(value='Salary')
        ttk.Combobox(frame, textvariable=category_var, values=["Needs", "Wants", "Culture", "Unplanned", "Salary"],
                     state="readonly", font=("Arial", 12)).grid(row=3, column=1, pady=5)

        # Description
        tk.Label(frame, text="Description:", font=("Arial", 12)).grid(row=4, column=0, sticky='e', pady=5)
        description_entry = tk.Entry(frame, font=("Arial", 12))
        description_entry.grid(row=4, column=1, pady=5)

        # Date
        tk.Label(frame, text="Date (YYYY-MM-DD):", font=("Arial", 12)).grid(row=5, column=0, sticky='e', pady=5)
        date_entry = tk.Entry(frame, font=("Arial", 12))
        date_entry.grid(row=5, column=1, pady=5)

        # Save Button
        save_button = tk.Button(frame, text="Save Transaction", command=save_transaction, font=("Arial", 12, "bold"),
                                bg="green", fg="white", width=20)
        save_button.grid(row=6, column=0, columnspan=2, pady=20)

        # Cancel Button
        cancel_button = tk.Button(frame, text="Cancel", command=top.destroy, font=("Arial", 12, "bold"), bg="red",
                                  fg="white", width=20)
        cancel_button.grid(row=7, column=0, columnspan=2, pady=5)

    def view_report(self):
        """ View a more interactive report with graphs """
        expense_categories = {"Needs": 0, "Wants": 0, "Culture": 0, "Unplanned": 0}
        for t in self.transactions:
            if t[0] == 'Expense':
                expense_categories[t[2]] += float(t[1])

        report_window = tk.Toplevel(self.root)
        report_window.title("KAKEIBO Monthly Reflection")

        # Get the position of the main window
        main_window_x = self.root.winfo_rootx()
        main_window_y = self.root.winfo_rooty()
        main_window_width = self.root.winfo_width()

        # Position the report window to the right of the main window
        report_window.geometry("+" + str(main_window_x + main_window_width + 10) + "+" + str(main_window_y))

        # Generate Pie Chart for Expenses
        labels = list(expense_categories.keys())
        sizes = list(expense_categories.values())
        plt.figure(figsize=(6, 6))
        plt.pie(sizes, labels=labels, autopct='%1.0f%%', startangle=90)
        plt.title("Expense Categories Distribution")

        # Add the pie chart to the Tkinter window
        canvas = FigureCanvasTkAgg(plt.gcf(), master=report_window)
        canvas.get_tk_widget().pack()
        canvas.draw()

    def manage_goals(self):
        """ Manage financial goals (add/edit) """
        goal_window = tk.Toplevel(self.root)
        goal_window.title("YOUR KAKEIBO GOALS")

        # Get the position of the main window
        main_window_x = self.root.winfo_rootx()
        main_window_y = self.root.winfo_rooty()
        main_window_width = self.root.winfo_width()

        # Position the goals window to the right of the main window
        goal_window.geometry("+" + str(main_window_x + main_window_width + 10) + "+" + str(main_window_y))

        # Create a frame to contain the listbox and buttons
        frame = tk.Frame(goal_window)
        frame.pack(padx=10, pady=10)

        # Label to instruct users
        label = tk.Label(frame, text="Saved Goals:", font=("Arial", 12))
        label.grid(row=0, column=0, pady=10)

        # Listbox to display saved goals
        goal_listbox = tk.Listbox(frame, height=10, width=50, font=("Arial", 10))
        goal_listbox.grid(row=1, column=0, pady=10)

        # Populate the Listbox with saved goals
        for goal in self.goals:
            goal_name, target_amount, target_date, amount_saved = goal
            goal_listbox.insert(tk.END, f"{goal_name}: ${target_amount} by {target_date} (Saved: ${amount_saved})")

        
        def refresh_goals():
            goal_listbox.delete(0, tk.END)
            for goal in self.goals:
                goal_name, target_amount, target_date, amount_saved = goal
                goal_listbox.insert(tk.END, f"{goal_name}: ${target_amount} by {target_date} (Saved: ${amount_saved})")


        # Button to add a new goal
        goal_name_label = tk.Label(frame, text="Goal Name:")
        goal_name_label.grid(row=3, column=0, pady=5)
        goal_name_entry = tk.Entry(frame)
        goal_name_entry.grid(row=3, column=1, pady=5)

        target_amount_label = tk.Label(frame, text="Target Amount ($):")
        target_amount_label.grid(row=4, column=0, pady=5)
        target_amount_entry = tk.Entry(frame)
        target_amount_entry.grid(row=4, column=1, pady=5)

        target_date_label = tk.Label(frame, text="Target Date (YYYY-MM-DD):")
        target_date_label.grid(row=5, column=0, pady=5)
        target_date_entry = tk.Entry(frame)
        target_date_entry.grid(row=5, column=1, pady=5)

        amount_saved_label = tk.Label(frame, text="Amount Saved ($):")
        amount_saved_label.grid(row=6, column=0, pady=5)
        amount_saved_entry = tk.Entry(frame)
        amount_saved_entry.grid(row=6, column=1, pady=5)

        def save_goal():
            goal_name = goal_name_entry.get()
            target_amount = target_amount_entry.get()
            target_date = target_date_entry.get()
            amount_saved = amount_saved_entry.get()

            if goal_name and target_amount and target_date and amount_saved:
                try:
                    target_amount = float(target_amount)
                    amount_saved = float(amount_saved)
                    datetime.strptime(target_date, "%Y-%m-%d")
                    self.goals.append([goal_name, target_amount, target_date, amount_saved])

                    # Save goal to CSV file
                    with open(self.goals_file, 'a', newline='') as file:
                        writer = csv.writer(file)
                        writer.writerow([goal_name, target_amount, target_date, amount_saved])

                    # Refresh the listbox and clear input fields
                    refresh_goals()
                    goal_name_entry.delete(0, tk.END)
                    target_amount_entry.delete(0, tk.END)
                    target_date_entry.delete(0, tk.END)
                    amount_saved_entry.delete(0, tk.END)

                except ValueError:
                    messagebox.showerror("Input Error", "Please provide valid numerical values.")

        save_button = tk.Button(frame, text="Save Goal", command=save_goal, width=20)
        save_button.grid(row=7, column=0, columnspan=2, pady=10)


if __name__ == "__main__":
    root = tk.Tk()
    app = FinancialApp(root)
    root.mainloop()
