# Eazybyts
index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>E-Learning Platform</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>Welcome to the E-Learning Platform</h1>
        <div class="buttons">
            <a href="login.html">Login</a>
            <a href="register.html">Register</a>
        </div>
    </div>
</body>
</html>

register.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Register</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>Register</h1>
        <form id="register-form">
            <input type="text" id="new-username" placeholder="Username" required>
            <input type="password" id="new-password" placeholder="Password" required>
            <select id="role">
                <option value="student">Student</option>
                <option value="instructor">Instructor</option>
            </select>
            <button type="submit">Register</button>
        </form>
    </div>
    <script src="scripts.js"></script>
</body>
</html>

login.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>Login</h1>
        <form id="login-form">
            <input type="text" id="username" placeholder="Username" required>
            <input type="password" id="password" placeholder="Password" required>
            <button type="submit">Login</button>
        </form>
    </div>
    <script src="scripts.js"></script>
</body>
</html>

dahboard.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>Dashboard</h1>
        <div id="dashboard-content"></div>
    </div>
    <script src="scripts.js"></script>
</body>
</html>

course.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Course</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>Course Title</h1>
        <div id="course-materials"></div>
        <button id="mark-complete">Mark as Complete</button>
    </div>
    <script src="scripts.js"></script>
</body>
</html>

styles.css
body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
    margin: 0;
    padding: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

.container {
    background-color: #fff;
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0 0 10px rgba(0,0,0,0.1);
    text-align: center;
}

h1 {
    margin-bottom: 20px;
}

input, select, button {
    display: block;
    width: 100%;
    margin-bottom: 10px;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
}
button {
    background-color: #28a745;
    color: #fff;
    border: none;
    cursor: pointer;
}

button:hover {
    background-color: #218838;
}

.buttons a {
    display: inline-block;
    padding: 10px 20px;
    margin: 5px;
    background-color: #007bff;
    color: #fff;
    text-decoration: none;
    border-radius: 5px;
}

.buttons a:hover {
    background-color: #0056b3;
}

scripts.js
// Sample user data storage
let users = JSON.parse(localStorage.getItem('users')) || [];
let currentUser = JSON.parse(localStorage.getItem('currentUser')) || null;

document.addEventListener('DOMContentLoaded', () => {
    // Handle registration
    if (document.getElementById('register-form')) {
        document.getElementById('register-form').addEventListener('submit', (e) => {
            e.preventDefault();
            const username = document.getElementById('new-username').value;
            const password = document.getElementById('new-password').value;
            const role = document.getElementById('role').value;
            users.push({ username, password, role, courses: [] });
            localStorage.setItem('users', JSON.stringify(users));
            alert('Registration successful');
            window.location.href = 'login.html';
        });
    }

    // Handle login
    if (document.getElementById('login-form')) {
        document.getElementById('login-form').addEventListener('submit', (e) => {
            e.preventDefault();
            const username = document.getElementById('username').value;
            const password = document.getElementById('password').value;
            currentUser = users.find(user => user.username === username && user.password === password);
            if (currentUser) {
                localStorage.setItem('currentUser', JSON.stringify(currentUser));
                window.location.href = 'dashboard.html';
            } else {
                alert('Invalid credentials');
            }
        });
    }

    // Load dashboard
    if (document.getElementById('dashboard-content')) {
        if (!currentUser) {
            window.location.href = 'login.html';
        } else {
            const contentDiv = document.getElementById('dashboard-content');
            contentDiv.innerHTML = <h2>Welcome, ${currentUser.username}</h2>;
            if (currentUser.role === 'instructor') {
                contentDiv.innerHTML += `
                    <form id="create-course-form">
                        <input type="text" id="course-title" placeholder="Course Title" required>
                        <button type="submit">Create Course</button>
                    </form>
                    <div id="instructor-courses"></div>
                `;
                document.getElementById('create-course-form').addEventListener('submit', (e) => {
                    e.preventDefault();
                    const title = document.getElementById('course-title').value;
                    if (!currentUser.courses) currentUser.courses = [];
                    currentUser.courses.push({ title, materials: [] });
                    localStorage.setItem('currentUser', JSON.stringify(currentUser));
                    displayCourses();
                });
                displayCourses();
            } else {
                displayCourses();
            }
        }
    }

    // Handle course completion
    if (document.getElementById('mark-complete')) {
        document.getElementById('mark-complete').addEventListener('click', () => {
            alert('Course marked as complete!');
        });
    }
});

function displayCourses() {
    const courseDiv = document.getElementById('instructor-courses') || document.getElementById('dashboard-content');
    if (courseDiv) {
        courseDiv.innerHTML = '<h3>Your Courses</h3>';
        currentUser.courses.forEach((course, index) => {
            courseDiv.innerHTML += <p>${course.title}</p>;
        });
    }
}
