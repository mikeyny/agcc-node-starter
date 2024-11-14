## Step-by-Step Tutorial: Building a Simple To-Do List App

### Prerequisites

- **Node.js** and **npm** installed
- Basic knowledge of **JavaScript**, **HTML**, and **Command Line**

In this tutorial, we will create a simple to-do list app using **Node.js**, **Express**, **Handlebars**, **SQLite**, and **Tailwind CSS** for styling. Follow along step-by-step, running each piece of code as we go.

### Step 1: Setting Up the Project

1. **Create a Project Folder**

   - Open your terminal and create a new folder for the project:
     ```bash
     mkdir todo-app
     cd todo-app
     ```

2. **Initialize the Project**

   - Run the following command to initialize a new Node.js project. This will create a `package.json` file:
     ```bash
     npm init -y
     ```

3. **Install Dependencies**

   - We need **Express** for our server, **Handlebars** for templating, **sqlite3** for the database, and **dotenv** for managing environment variables:
     ```bash
     npm install express express-handlebars sqlite3 dotenv
     ```

### Step 2: Setting Up a Basic HTTP Server

1. **Create the Basic Server**

   - In the project root, create a new file called `app.js`:
     ```bash
     touch app.js
     ```
   - Add the following code to `app.js` to create a basic Express server:
     ```javascript
     const express = require("express");
     const app = express();

     app.get("/", (req, res) => {
       res.send("Hello World");
     });

     const PORT = process.env.PORT || 3000;
     app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
     ```
   - This sets up a basic Express server that listens on port 3000 and returns "Hello World" for the root route (`/`).
   -

2. **Run the Server**

   - Start the server by running:
     ```bash
     node app.js
     ```
   - Open your browser and go to `http://localhost:3000` to verify that your server is running and returning "Hello World".

### Step 3: Serving a Basic HTML Page

1. **Create a Public Directory**
   - Create a directory named `public` to store your static files (such as HTML, CSS, images, and JavaScript):
     ```bash
     mkdir public
     ```

2. **Add a Basic HTML File**
   - Create a file named `index.html` in the `public` folder:
     ```bash
     touch public/index.html
     ```
   - Add the following simple HTML code to `index.html`:
     ```html
     <!DOCTYPE html>
     <html lang="en">
     <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <link href="style.css" rel="stylesheet">
       <title>Basic HTML Page</title>
     </head>
     <body>
       <h1>Welcome to My Basic HTML Page</h1>
       <p>This is a simple HTML page served by Express.</p>
     </body>
     </html>
     ```

3. **Add a Static CSS File**
   - Create a file named `style.css` in the `public` folder to style your HTML page:
     ```bash
     touch public/style.css
     ```
   - Add the following simple CSS code to `style.css`:
     ```css
     body {
       font-family: Arial, sans-serif;
       background-color: #f0f0f0;
     }

     h1 {
       color: #333;
     }
     ```

4. **Serve Static Files in Express**
   - Update `app.js` to serve static files from the `public` directory:
     ```javascript
     const path = require("path");

     app.use(express.static(path.join(__dirname, "public")));
     ```

5. **Run the Server**
   - Restart the server:
     ```bash
     node app.js
     ```
   - Open your browser and go to `http://localhost:3000` to see the HTML page being served correctly.

### Step 3: Using a Templating Engine (Handlebars)

1. **Install Handlebars**

   - Install **express-handlebars** to set up the templating engine:
     ```bash
     npm install express-handlebars
     ```

2. **Set Up Handlebars in Express**

   - Update `app.js` to use Handlebars as the templating engine:
     ```javascript
     const handlebars = require("express-handlebars");

     app.engine("hbs", handlebars({ extname: ".hbs" }));
     app.set("view engine", "hbs");
     app.set("views", path.join(__dirname, "views"));

     app.use(express.urlencoded({ extended: true }));
     ```

3. **Create the Folder Structure**

   - Create the necessary folders and files:
     ```bash
     mkdir views views/layouts public
     touch views/layouts/main.hbs views/index.hbs
     ```

4. **Create the Main Layout**

   - Add the following HTML to `views/layouts/main.hbs`:
     ```html
     <!DOCTYPE html>
     <html lang="en">
     <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
       <title>To-Do List</title>
     </head>
     <body class="bg-gray-100">
       {{{body}}}
     </body>
     </html>
     ```

5. **Create the Home Page Template**

   - Add the following code to `views/index.hbs`:
     ```html
     <div class="container mx-auto p-4">
       <h1 class="text-3xl font-bold mb-4">To-Do List</h1>
       <form action="/add" method="POST" class="mb-4">
         <input type="text" name="task" placeholder="New task" class="border p-2 mr-2 rounded">
         <button type="submit" class="bg-blue-500 text-white p-2 rounded">Add Task</button>
       </form>
       <ul>
         {{#each tasks}}
         <li class="mb-2">
           {{this.task}}
           <a href="/delete/{{this.id}}" class="text-red-500 ml-4">Delete</a>
         </li>
         {{/each}}
       </ul>
     </div>
     ```

6. **Run the Server**

   - Start the server again by running:
     ```bash
     node app.js
     ```
   - Open your browser and go to `http://localhost:3000` to see the basic layout of the to-do list.

### Step 4: Creating a Basic To-Do List

1. **Add Routes for CRUD Operations**

   - Open `app.js` and add the following code to set up routes for the to-do list:
     ```javascript
     app.get("/", (req, res) => {
       res.render("index", { tasks: [] });
     });

     app.post("/add", (req, res) => {
       const task = req.body.task;
       tasks.push({ id: tasks.length + 1, task });
       res.redirect("/");
     });

     app.get("/delete/:id", (req, res) => {
       const id = parseInt(req.params.id);
       tasks = tasks.filter(task => task.id !== id);
       res.redirect("/");
     });
     ```
   - **Note**: In this step, we are using an in-memory array (`tasks`) to store the to-do items temporarily.

2. **Run the Server**

   - Start the server by running:
     ```bash
     node app.js
     ```
   - Go to `http://localhost:3000` to add and delete tasks.

### Step 5: Storing To-Do Items in SQLite Database

1. **Create a Database File**

   - Create a file named `database.js` in the project root:
     ```bash
     touch database.js
     ```
   - Add the following code to `database.js` to set up a simple SQLite database:
     ```javascript
     const sqlite3 = require("sqlite3").verbose();
     const db = new sqlite3.Database("tasks.db");

     db.serialize(() => {
       db.run("CREATE TABLE IF NOT EXISTS tasks (id INTEGER PRIMARY KEY, task TEXT)");
     });

     module.exports = db;
     ```

2. **Update CRUD Operations to Use SQLite**

   - Update `app.js` to use the SQLite database:
     ```javascript
     const db = require("./database");

     app.get("/", (req, res) => {
       db.all("SELECT * FROM tasks", (err, rows) => {
         if (err) {
           return res.status(500).send("Error retrieving tasks");
         }
         res.render("index", { tasks: rows });
       });
     });

     app.post("/add", (req, res) => {
       const { task } = req.body;
       db.run("INSERT INTO tasks (task) VALUES (?)", task, (err) => {
         if (err) {
           return res.status(500).send("Error adding task");
         }
         res.redirect("/");
       });
     });

     app.get("/delete/:id", (req, res) => {
       const { id } = req.params;
       db.run("DELETE FROM tasks WHERE id = ?", id, (err) => {
         if (err) {
           return res.status(500).send("Error deleting task");
         }
         res.redirect("/");
       });
     });
     ```

3. **Run the Server**

   - Start the server by running:
     ```bash
     node app.js
     ```
   - Go to `http://localhost:3000` to see your to-do list app with persistent storage using SQLite.

### Conclusion

You now have a fully functional to-do list app with a backend powered by **Express** and **SQLite**, dynamic views using **Handlebars**, and styling from **Tailwind CSS**. Feel free to extend this app further by adding features like editing tasks or marking them as complete.


