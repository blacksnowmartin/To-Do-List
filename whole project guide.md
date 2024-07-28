Step-by-Step Guide
Step 1: Setting Up the Project Directory
Create the project directory and navigate into it:

sh
Copy code
mkdir to_do_list_app
cd to_do_list_app
Set up a virtual environment:

sh
Copy code
python3 -m venv venv
source venv/bin/activate
Create necessary files and directories:

sh
Copy code
mkdir app
mkdir app/static
mkdir app/templates
touch app/__init__.py app/models.py app/routes.py run.py requirements.txt .gitignore
Step 2: Install Dependencies
requirements.txt:

txt
Copy code
Flask==2.0.1
sh
Copy code
pip install -r requirements.txt
Step 3: Initialize the Flask App
app/init.py:

python
Copy code
from flask import Flask

app = Flask(__name__)

from app import routes
run.py:

python
Copy code
from app import app

if __name__ == "__main__":
    app.run(debug=True)
Step 4: Create Models
app/models.py:

python
Copy code
tasks = []
Step 5: Define Routes
app/routes.py:

python
Copy code
from flask import render_template, request, redirect, url_for
from app import app
from app.models import tasks

@app.route("/")
def index():
    return render_template("index.html", tasks=tasks)

@app.route("/add", methods=["POST"])
def add_task():
    task = request.form.get("task")
    if task:
        tasks.append({"task": task, "completed": False})
    return redirect(url_for("index"))

@app.route("/delete/<int:task_id>")
def delete_task(task_id):
    if 0 <= task_id < len(tasks):
        del tasks[task_id]
    return redirect(url_for("index"))

@app.route("/complete/<int:task_id>")
def complete_task(task_id):
    if 0 <= task_id < len(tasks):
        tasks[task_id]["completed"] = True
    return redirect(url_for("index"))
Step 6: Create Templates
app/templates/index.html:

html
Copy code
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>To-Do List</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='styles.css') }}">
</head>
<body>
    <h1>To-Do List</h1>
    <form action="{{ url_for('add_task') }}" method="POST">
        <input type="text" name="task" placeholder="Enter a new task">
        <button type="submit">Add</button>
    </form>
    <ul>
        {% for idx, task in enumerate(tasks) %}
            <li>
                {{ task.task }} - {{ 'Completed' if task.completed else 'Pending' }}
                <a href="{{ url_for('complete_task', task_id=idx) }}">Complete</a>
                <a href="{{ url_for('delete_task', task_id=idx) }}">Delete</a>
            </li>
        {% endfor %}
    </ul>
</body>
</html>
Step 7: Add Styles and Scripts
app/static/styles.css:

css
Copy code
body {
    font-family: Arial, sans-serif;
    max-width: 600px;
    margin: 0 auto;
    padding: 20px;
}
ul {
    list-style-type: none;
    padding: 0;
}
li {
    margin: 10px 0;
}
a {
    margin-left: 10px;
}
app/static/scripts.js: (optional, if you want to add some JS functionality later)

javascript
Copy code
// You can add JavaScript here if needed
Step 8: Initialize Git and Create a GitHub Repository
Initialize a git repository:

sh
Copy code
git init
Add all files and commit:

sh
Copy code
git add .
git commit -m "Initial commit"
Create a new repository on GitHub and follow the instructions to add the remote origin and push your code:

sh
Copy code
git remote add origin https://github.com/YOUR_USERNAME/to_do_list_app.git
git branch -M main
git push -u origin main
Replace YOUR_USERNAME with your GitHub username and to_do_list_app with your repository name.

Final Notes
Once you've followed these steps, your To-Do List app should be up and running. You can access it by running:

sh
Copy code
python run.py
Open your browser and navigate to http://127.0.0.1:5000 to see your app in action.

Feel free to customize and expand this project as needed. If you need further assistance or have specific requirements, let me know!
