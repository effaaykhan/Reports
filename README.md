# PROJECT REPORTS
  - This repo includes the final project reports for Vulnerability Assessment on [testphp.vulnweb](http://testphp.vulnweb.com/) and Hacking Cycle on Windows 7 and Ubuntu systems.
## Vulnerability Assessment
  - In this session i've learnt the various to attack any website and grab the useful credentials. In the report i've mentioned multiple methods in detail.
    1. Cross-Site Scripting (XSS)
    2. SQL Injection
    3. Outdated PHP versions
    4. Weak Password
### Cross-Site Scripting (XSS)
  - Cross-Site Scripting (XSS) is a type of computer security vulnerability typically found in web applications. XSS enables attackers to inject client-side scripts into web pages viewed by other users. A cross-site scripting vulnerability may be used by attackers to bypass access controls such as the Same Origin Policy.

  Here's an example of how XSS can be exploited:
  
  1. An attacker finds a website that accepts user input and displays the input in an unsafe manner.
  2. The attacker crafts a URL with a malicious script, such as:
     ```
     http://example.com/search?query=<script>alert('XSS')</script>
     ```
  3. The user clicks on the malicious link and their browser executes the script.
  4. The script alerts the user with a message, or could potentially steal sensitive information from the user's session.
  
  To prevent XSS, web applications should:
  - Sanitize and validate all user input before displaying it on the page.
  - Use a safe templating system or framework that automatically escapes user input.
  - Set the `Content-Security-Policy` header to restrict the sources of scripts that can be loaded.
  - Use `HttpOnly` cookies to prevent client-side access to sensitive data.
  
  Here's an example of how to properly sanitize user input in PHP:
  
  ```php
  <?php
  $userInput = $_GET['userInput'];
  
  // Sanitize the user input
  $sanitizedInput = htmlspecialchars($userInput, ENT_QUOTES, 'UTF-8');
  
  // Display the sanitized input
  echo "<div>" . $sanitizedInput . "</div>";
  ?>
  ```
  
  This code uses the `htmlspecialchars` function to sanitize the user input and escape any HTML entities. The `ENT_QUOTES` flag ensures that both single and double quotes are escaped, preventing the user input from breaking out of the HTML context.
  
  By properly sanitizing user input, XSS attacks can be prevented.

### SQL Injection
  - SQL injection is a type of computer security vulnerability that allows an attacker to manipulate a SQL query sent to a database. It typically occurs when user input is directly used in a SQL query without proper sanitization.

  Here's an example of how SQL injection can be exploited:
  
  1. An attacker finds a website that accepts user input and constructs a SQL query based on that input.
  2. The attacker crafts a malicious input, such as: `' OR 1=1 --`
  3. The application's SQL query is constructed as:
     ```sql
     SELECT * FROM users WHERE username = '$userInput' AND password = '$passwordInput'
     ```
     The attacker's input is: `' OR 1=1 --`
  4. The resulting SQL query becomes:
     ```sql
     SELECT * FROM users WHERE username = '' OR 1=1 --' AND password = ''
     ```
     The `OR 1=1` clause always evaluates to true, and the `--` comments out the rest of the query.
  5. The application executes this query, allowing the attacker to bypass authentication and access sensitive data.
  
  To prevent SQL injection, web applications should:
  - Use parameterized queries or prepared statements to separate user input from the SQL query.
  - Sanitize and validate all user input before using it in SQL queries.
  - Use a safe API for interacting with the database, such as PDO or mysqli, which automatically escapes user input.
  - Set the `Content-Security-Policy` header to restrict the sources of scripts that can be loaded.
  
  Here's an example of how to properly use parameterized queries in PHP with PDO:
  
  ```php
  <?php
  $userInput = $_GET['userInput'];
  
  // Prepare the SQL query
  $query = "SELECT * FROM users WHERE username = :username AND password = :password";
  
  // Prepare the statement
  $stmt = $pdo->prepare($query);
  
  // Bind parameters
  $stmt->bindParam(':username', $userInput);
  $stmt->bindParam(':password', $passwordInput);
  
  // Execute the query
  $stmt->execute();
  
  // Fetch the results
  $results = $stmt->fetchAll();
  ?>
  ```
  
  In this code, the user input is bound to the query parameters using `bindParam`, which ensures that the input is treated as a parameter and not part of the actual SQL query. This prevents SQL injection.
  
  By using parameterized queries and properly sanitizing user input, SQL injection can be prevented.


## Hacking Cycle (Windows & Ubuntu)
  - In this project I break down into the windows 7 and ubuntu OSs to get the login password. I used many vulnerabilites and exploits that I found in these systems and exploited them to get the login password in hash format. I used john the ripper tool to crack the hash.
