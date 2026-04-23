Great—PHP is a solid choice, especially for web development. I’ll walk you from beginner to advanced in a structured way so you actually *build skill*, not just read syntax.

---

# 🧱 PHASE 1: Basics (Foundation)

### 1. What is PHP?

* PHP = server-side scripting language
* Runs on a server (like Apache or Nginx)
* Used to build dynamic websites (forms, logins, dashboards)

### 2. Your First PHP Script

```php
<?php
echo "Hello, World!";
?>
```

### 3. Variables & Data Types

```php
$name = "John";   // string
$age = 20;        // integer
$price = 10.5;    // float
$isStudent = true; // boolean
```

### 4. Conditionals

```php
if ($age >= 18) {
    echo "Adult";
} else {
    echo "Minor";
}
```

### 5. Loops

```php
for ($i = 0; $i < 5; $i++) {
    echo $i;
}
```

---

# ⚙️ PHASE 2: Intermediate PHP

### 1. Functions

```php
function greet($name) {
    return "Hello " . $name;
}

echo greet("John");
```

### 2. Arrays

```php
$fruits = ["apple", "banana", "orange"];

foreach ($fruits as $fruit) {
    echo $fruit;
}
```

### 3. Forms (Very Important)

HTML:

```html
<form method="POST">
    <input type="text" name="username">
    <button type="submit">Submit</button>
</form>
```

PHP:

```php
$username = $_POST['username'];
echo $username;
```

### 4. Superglobals

* `$_GET`
* `$_POST`
* `$_SESSION`
* `$_COOKIE`

---

# 🗄️ PHASE 3: Working with Databases (MySQL)

### Connect to Database

```php
$conn = new mysqli("localhost", "root", "", "test");

if ($conn->connect_error) {
    die("Connection failed");
}
```

### Insert Data

```php
$sql = "INSERT INTO users (name) VALUES ('John')";
$conn->query($sql);
```

### Fetch Data

```php
$result = $conn->query("SELECT * FROM users");

while ($row = $result->fetch_assoc()) {
    echo $row['name'];
}
```

---

# 🔐 PHASE 4: Advanced Concepts

### 1. Sessions (Login systems)

```php
session_start();
$_SESSION['user'] = "John";
```

### 2. Password Hashing

```php
$password = password_hash("123456", PASSWORD_DEFAULT);
```

### 3. OOP (Object-Oriented PHP)

```php
class User {
    public $name;

    function __construct($name) {
        $this->name = $name;
    }

    function greet() {
        return "Hello " . $this->name;
    }
}
```

---

# 🚀 PHASE 5: Pro Level

### 1. MVC Structure

* Model → database logic
* View → UI
* Controller → logic

### 2. Composer (Dependency Manager)

Used to install packages

### 3. REST APIs

```php
header("Content-Type: application/json");
echo json_encode(["name" => "John"]);
```

---

# 🧱 1. Setup (Simple API File)

Create a file: `api.php`

```php
<?php
header("Content-Type: application/json");

$conn = new mysqli("localhost", "root", "", "test");

if ($conn->connect_error) {
    die(json_encode(["error" => "Database connection failed"]));
}

$method = $_SERVER['REQUEST_METHOD'];
```

---

# ➕ 2. CREATE (POST)

### Postman Setup:

* Method: **POST**
* URL: `http://localhost/api.php`
* Body → raw → JSON:

```json
{
  "name": "John"
}
```

### PHP:

```php
if ($method == 'POST') {
    $data = json_decode(file_get_contents("php://input"), true);
    $name = $data['name'];

    $stmt = $conn->prepare("INSERT INTO users (name) VALUES (?)");
    $stmt->bind_param("s", $name);
    $stmt->execute();

    echo json_encode(["message" => "User created"]);
}
```

---

# 📥 3. READ (GET)

### Postman Setup:

* Method: **GET**
* URL: `http://localhost/api.php`

### PHP:

```php
if ($method == 'GET') {
    $result = $conn->query("SELECT * FROM users");

    $users = [];
    while ($row = $result->fetch_assoc()) {
        $users[] = $row;
    }

    echo json_encode($users);
}
```

---

# ✏️ 4. UPDATE (PUT)

### Postman Setup:

* Method: **PUT**
* URL: `http://localhost/api.php`
* Body:

```json
{
  "id": 1,
  "name": "Updated Name"
}
```

### PHP:

```php
if ($method == 'PUT') {
    $data = json_decode(file_get_contents("php://input"), true);

    $id = $data['id'];
    $name = $data['name'];

    $stmt = $conn->prepare("UPDATE users SET name=? WHERE id=?");
    $stmt->bind_param("si", $name, $id);
    $stmt->execute();

    echo json_encode(["message" => "User updated"]);
}
```

---

# ❌ 5. DELETE

### Postman Setup:

* Method: **DELETE**
* URL: `http://localhost/api.php`
* Body:

```json
{
  "id": 1
}
```

### PHP:

```php
if ($method == 'DELETE') {
    $data = json_decode(file_get_contents("php://input"), true);

    $id = $data['id'];

    $stmt = $conn->prepare("DELETE FROM users WHERE id=?");
    $stmt->bind_param("i", $id);
    $stmt->execute();

    echo json_encode(["message" => "User deleted"]);
}
```

---

# 🌍 1. Example: Calling an External API

We’ll use a free public API:
👉 JSONPlaceholder

---

# ⚙️ 2. Simple GET Request (External API)

Add this to your `api.php`:

```php
if ($method == 'GET' && isset($_GET['external'])) {

    $url = "https://jsonplaceholder.typicode.com/users";

    $response = file_get_contents($url);

    if ($response === FALSE) {
        echo json_encode(["error" => "Failed to fetch external API"]);
        exit;
    }

    echo $response;
}
```

---

# 🧪 Test in Postman

* Method: **GET**
* URL:

```
http://localhost/api.php?external=1
```

You’ll get users from the external API.

---

# 🔐 3. Better Way (Using cURL)

`file_get_contents` works, but **cURL is more powerful**.

```php
if ($method == 'GET' && isset($_GET['external'])) {

    $ch = curl_init();

    curl_setopt($ch, CURLOPT_URL, "https://jsonplaceholder.typicode.com/users");
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

    $response = curl_exec($ch);

    if (curl_errno($ch)) {
        echo json_encode(["error" => curl_error($ch)]);
    } else {
        echo $response;
    }

    curl_close($ch);
}
```

---

# 📤 4. Sending Data to External API (POST)

```php
if ($method == 'POST' && isset($_GET['external'])) {

    $data = [
        "name" => "John",
        "email" => "john@example.com"
    ];

    $ch = curl_init("https://jsonplaceholder.typicode.com/users");

    curl_setopt($ch, CURLOPT_POST, true);
    curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode($data));
    curl_setopt($ch, CURLOPT_HTTPHEADER, [
        "Content-Type: application/json"
    ]);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

    $response = curl_exec($ch);

    curl_close($ch);

    echo $response;
}
```
---

# ⚖️ PHP vs C#

### 🧠 Typing

* PHP → **Dynamically typed**
* C# → **Strongly & statically typed**

### Example:

**PHP:**

```php
$x = 10;
$x = "hello"; // totally fine
```

**C#:**

```csharp
int x = 10;
x = "hello"; // ❌ error
```
---

# 📦 Arrays in PHP (VERY IMPORTANT)

PHP arrays are not like C# arrays.

They are actually:
👉 **Ordered maps (mix of array + dictionary)**

```php
$numbers = [1, 2, 3];

$user = [
    "name" => "John",
    "age" => 20
];
```

👉 That second one is like a **Dictionary in C#**

---

# 🔁 Looping Arrays

```php
foreach ($user as $key => $value) {
    echo $key . ": " . $value;
}
```

---

# 🧱 Objects in PHP

PHP supports OOP like C#, but it's less strict.

```php
class User {
    public $name;

    function __construct($name) {
        $this->name = $name;
    }
}

$user = new User("John");
echo $user->name;
```

---

# ⚡ Arrays vs Objects in PHP

This part is where beginners get confused:

### Array:

```php
$user = ["name" => "John"];
echo $user["name"];
```

### Object:

```php
$user = (object)["name" => "John"];
echo $user->name;
```

👉 Same data, different access style.

---

# 🔄 JSON → Array vs Object

When working with APIs (like in Postman):

```php
$data = json_decode($json, true); // array
echo $data["name"];
```

```php
$data = json_decode($json); // object
echo $data->name;
```

👉 The `true` decides everything.

---
---

```php
# data type
<?php 

// scalar
 $name="follyb';
 echo "follyb";
 echo "<p>follyb</p>";
 echo $name;
 $age = 10;
 echo $age;

 // array
 $names = array("a","b","c")

 $numbers = [1, 2, 3];
// Dictionary
$user = [
    "name" => "John",
    "age" => 20
];

//class
class User {
    public $name;

    function __construct($name) {
        $this->name = $name;
    }
}

$user = new User("John");
echo $user->name;
?>
```