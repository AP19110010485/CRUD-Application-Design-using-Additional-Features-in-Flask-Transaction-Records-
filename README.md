📝 Flask Transaction Manager
A simple Flask application to manage financial transactions with full CRUD functionality.

🚀 Project Setup
📦 1. Clone the Repository
```
git clone https://github.com/ibm-developer-skills-network/obmnl-flask_assignment.git
cd obmnl-flask_assignment
```
Your project structure should now look like this:

```
obmnl-flask_assignment/
├── app.py
└── templates/
    ├── edit.html
    ├── form.html
    └── transactions.html
```
🔧 2. Initial Setup
✅ Import Flask Functions in app.py
At the top of app.py, import the required modules and create the Flask app:

```
from flask import Flask, redirect, request, render_template, url_for

app = Flask(__name__)
```
🧪 Add Sample Data (Optional)
Add sample transactions to test the interface:

```
transactions = [
    {'id': 1, 'date': '2023-06-01', 'amount': 100},
    {'id': 2, 'date': '2023-06-02', 'amount': -200},
    {'id': 3, 'date': '2023-06-03', 'amount': 300}
]
```
📖 3. CRUD Operations
📄 Read Operation
Display all transactions at the root URL /:

```
@app.route("/")
def get_transactions():
    return render_template("transactions.html", transactions=transactions)
```
➕ Create Operation
Handle GET to show the form and POST to save a new transaction:

```
@app.route("/add", methods=["GET", "POST"])
def add_transaction():
    if request.method == 'POST':
        transaction = {
            'id': len(transactions) + 1,
            'date': request.form['date'],
            'amount': float(request.form['amount'])
        }
        transactions.append(transaction)
        return redirect(url_for("get_transactions"))
    return render_template("form.html")
```
✏️ Update Operation
Edit an existing transaction based on its ID:

```
@app.route("/edit/<int:transaction_id>", methods=["GET", "POST"])
def edit_transaction(transaction_id):
    if request.method == 'POST':
        date = request.form['date']
        amount = float(request.form['amount'])
        for transaction in transactions:
            if transaction['id'] == transaction_id:
                transaction['date'] = date
                transaction['amount'] = amount
                break
        return redirect(url_for("get_transactions"))

    for transaction in transactions:
        if transaction['id'] == transaction_id:
            return render_template("edit.html", transaction=transaction)
    return {"message": "Transaction not found"}, 404
```
🗑️ Delete Operation
Remove a transaction based on its ID:

```
@app.route("/delete/<int:transaction_id>")
def delete_transaction(transaction_id):
    for transaction in transactions:
        if transaction['id'] == transaction_id:
            transactions.remove(transaction)
            break
    return redirect(url_for("get_transactions"))
```
🏁 Final Step: Run the Application
Add this block at the end of app.py to start the server:

```
if __name__ == "__main__":
    app.run(debug=True)
```
Then start the Flask server by running:

```
python3.11 app.py
```
🌐 Access the App
Once running, visit your app in the browser at:

```
http://localhost:5000
```
If you're using an online IDE (like Skills Network Labs), launch the app and enter port 5000 when prompted.
