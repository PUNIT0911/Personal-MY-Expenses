# Personal-MY-Expenses
import streamlit as st
import pandas as pd

# Title
st.title("Expense Tracker")

# Sidebar for input
st.sidebar.header("Add a New Expense")
date = st.sidebar.date_input("Date")
category = st.sidebar.selectbox("Category", ["Food", "Transport", "Entertainment", "Other"])
amount = st.sidebar.number_input("Amount", min_value=0.0, step=1.0)
description = st.sidebar.text_input("Description")
add_expense = st.sidebar.button("Add Expense")

# Initialize session state for expense data
if "expenses" not in st.session_state:
    st.session_state.expenses = pd.DataFrame(columns=["Date", "Category", "Amount", "Description"])

# Add new expense to the session state
if add_expense:
    new_expense = pd.DataFrame([[date, category, amount, description]], 
                                columns=["Date", "Category", "Amount", "Description"])
    st.session_state.expenses = pd.concat([st.session_state.expenses, new_expense], ignore_index=True)
    st.success("Expense added!")

# Display current expenses
st.subheader("Current Expenses")
if not st.session_state.expenses.empty:
    st.dataframe(st.session_state.expenses)

    # Total expenses
    total = st.session_state.expenses["Amount"].sum()
    st.write(f"### Total Expense: ${total}")

    # Download as CSV
    csv = st.session_state.expenses.to_csv(index=False).encode('utf-8')
    st.download_button("Download Expenses as CSV", csv, "expenses.csv", "text/csv")
else:
    st.write("No expenses added yet.")
