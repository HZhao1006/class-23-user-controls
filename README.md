# INFO 2300

Open this repository as a Codespace on GitHub (or as a container in VS Code.)

## Development Server

From the **Run** menu, select **Start Debugging**.

Visit <http://127.0.0.1:8080/> in a browser to access the development server.

## Activity: Part I - Sessions

1. Create `users` and `sessions` tables in the database.

    ```sql
    --- Users ---
    CREATE TABLE users (
      id INTEGER NOT NULL UNIQUE,
      username TEXT NOT NULL UNIQUE,
      password TEXT NOT NULL,
      PRIMARY KEY(id AUTOINCREMENT)
    );

    INSERT INTO
      users (id, username, password)
    VALUES
      (
        1,
        'username',
        '$2y$10$QtCybkpkzh7x5VN11APHned4J8fu78.eFXlyAMmahuAaNcbwZ7FH.' -- monkey
      );

    --- Sessions ---
    CREATE TABLE sessions (
      id INTEGER NOT NULL UNIQUE,
      user_id INTEGER NOT NULL,
      session TEXT NOT NULL UNIQUE,
      last_login TEXT NOT NULL,
      PRIMARY KEY(id AUTOINCREMENT) FOREIGN KEY(user_id) REFERENCES users(id)
    );
    ```

2. Import the `sessions.php` library into your repository.

    Check the `includes` folder. The `sessions.php` file has already been added for you.

3. For every _page_ request, load the `sessions.php` library and check the HTTP request for session parameters _after_ initializing the database.

    ```php
    // check login/logout params
    require_once "includes/sessions.php";
    $session_messages = array();
    process_session_params($db, $session_messages);
    ```

4. Add a login form to the home page.

    ```php
    <?php echo login_form('/', $session_messages); ?>
    ```

    **Note:** The first parameter (i.e. `'/'`) is the URL to redirect to after a successful login.

5. Add a logout link to the navigation bar.

    ```php
    <?php echo logout_url(); ?>
    ```

    **Note:** The course example's logout link in the navigation bar is an extremely poor design choice for your project 3.

## Activity: Part II - User Access Controls

1. Show a welcome message on the home page **only** when the user is logged in.

    ```php
    <p>Welcome <strong><?php echo htmlspecialchars($current_user['username']); ?></strong>!</p>
    ```

    **Hint:** `if (is_user_logged_in()) { ... }`

2. Only show the login form on the home page when the user is not logged in.

3. Only show the logout link in the navigation bar when the user is logged in.
