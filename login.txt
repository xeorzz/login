Login.php

<?php
session_start();

// Check if the user is already logged in
if (isset($_SESSION["loggedin"]) && $_SESSION["loggedin"] === true) {
header("location: welcome.php");
exit;
}

// Hardcoded credentials for the sake of example
$myUsername = "user";
$myPassword = "password";

// Processing form data when form is submitted
if ($_SERVER["REQUEST_METHOD"] == "POST") {

$username = $_POST["username"];
$password = $_POST["password"];

// Check if username and password are correct
if ($username == $myUsername && $password == $myPassword) {
// Password is correct, start a new session
session_start();

// Store data in session variables
$_SESSION["loggedin"] = true;
$_SESSION["username"] = $username;

// Redirect user to welcome page
header("location: welcome.php");
} else {
// Display an error message if password is not valid
$login_err = "Invalid username or password.";
}
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Login</title>
</head>
<body>
<div>
<h2>Login</h2>
<p>Please fill in your credentials to login.</p>

<?php
if (!empty($login_err)) {
echo '<div>' . $login_err . '</div>';
}
?>

<form action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]); ?>" method="post">
<div>
<label>Username</label>
<input type="text" name="username">
</div>
<div>
<label>Password</label>
<input type="password" name="password">
</div>
<div>
<input type="submit" value="Login">
</div>
</form>
</div>
</body>
</html>


welcome.php

<?php
session_start();

// Check if the user is logged in, if not redirect to login page
if(!isset($_SESSION["loggedin"]) || $_SESSION["loggedin"] !== true){
header("location: login.php");
exit;
}

?>

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Welcome</title>
</head>
<body>
<div>
<h1>Welcome, <b><?php echo htmlspecialchars($_SESSION["username"]); ?></b></h1>
</div>
<p>
<a href="logout.php">Logout</a>
</p>
</body>
</html>


logout.php

<?php
// Initialize the session
session_start();

// Unset all of the session variables
$_SESSION = array();

// Destroy the session.
session_destroy();

// Redirect to login page
header("location: login.php");
exit;
?>
