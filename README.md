# AuthNest

**AuthNest** is a Node.js-based authentication system that leverages Passport.js for handling both local and Google OAuth2 authentication. The project uses Express.js for the server, PostgreSQL for the database, bcrypt for password hashing, and sessions to maintain user login states. This README file will guide you through setting up the project, understanding its structure, and the technologies used.

## Table of Contents

- [Features](#features)
- [Technologies Used](#technologies-used)
- [Installation](#installation)
- [Environment Variables](#environment-variables)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Key Components](#key-components)
  - [Session Management](#session-management)
  - [Authentication](#authentication)
  - [Google OAuth2 Integration](#google-oauth2-integration)
  - [Password Hashing](#password-hashing)
- [License](#license)

## Features

- **Local Authentication**: Users can register and log in using their email and password.
- **Google OAuth2 Authentication**: Users can log in using their Google account.
- **Session Management**: User sessions are maintained using Express sessions.
- **Password Security**: Passwords are securely hashed using bcrypt.
- **PostgreSQL Integration**: User data is stored in a PostgreSQL database.

## Technologies Used

- **Node.js**: JavaScript runtime for building the backend.
- **Express.js**: Web framework for Node.js.
- **Passport.js**: Middleware for authentication in Node.js.
- **PostgreSQL**: Relational database for storing user data.
- **bcrypt**: Library for hashing passwords.
- **dotenv**: Module for loading environment variables.
- **EJS**: Templating engine for rendering HTML.

## Installation

1. **Clone the repository:**

    ```bash
    git clone https://github.com/YogeshShrivas23
    cd AuthNest
    ```

2. **Install dependencies:**

    ```bash
    npm install
    ```

3. **Set up PostgreSQL:**

   Ensure you have PostgreSQL installed and set up a database for this project. Update the `.env` file with your PostgreSQL credentials.

4. **Create and Configure `.env` file:**

   Create a `.env` file in the root directory with the following environment variables:

    ```plaintext
    SESSION_SECRET=your_session_secret
    DB_USER=your_db_user
    DB_HOST=your_db_host
    DB_DATABASE=your_db_name
    DB_PASSWORD=your_db_password
    DB_PORT=your_db_port
    GOOGLE_CLIENT_ID=your_google_client_id
    GOOGLE_CLIENT_SECRET=your_google_client_secret
    ```

5. **Run the application:**

    ```bash
    npm start
    ```

6. **Visit the Application:**

   Open your browser and visit `http://localhost:3000`.

## Environment Variables

Ensure you have the following environment variables configured in your `.env` file:

- `SESSION_SECRET`: A secret key for signing session cookies.
- `DB_USER`: Your PostgreSQL username.
- `DB_HOST`: Host address for your PostgreSQL database.
- `DB_DATABASE`: Name of your PostgreSQL database.
- `DB_PASSWORD`: Password for your PostgreSQL user.
- `DB_PORT`: Port number for your PostgreSQL database.
- `GOOGLE_CLIENT_ID`: Your Google OAuth2 client ID.
- `GOOGLE_CLIENT_SECRET`: Your Google OAuth2 client secret.

## Usage

- **Home Page:** The root route (`/`) renders the home page.
- **Login Page:** The `/login` route renders the login form.
- **Registration Page:** The `/register` route renders the registration form.
- **Dashboard:** The `/page` route is protected and requires authentication. It redirects to the login page if the user is not authenticated.
- **Google Authentication:** The `/auth/google` route initiates the Google OAuth2 login flow.
- **Logout:** The `/logout` route logs out the user and redirects to the home page.

## Project Structure

```plaintext
├── public/               # Static files (CSS, JS, images)
├── views/                # EJS templates (home.ejs, login.ejs, register.ejs, page.ejs)
├── .env                  # Environment variables
├── index.js              # Main server file
├── package.json          # Project metadata and dependencies
└── README.md             # Project documentation
```

## Key Components

### Session Management

- **Session**: The app uses `express-session` to manage user sessions. Sessions are stored in a cookie on the client's browser, allowing the server to recognize returning users.
  
    ```javascript
    app.use(
      session({
        secret: process.env.SESSION_SECRET,
        resave: false,
        saveUninitialized: true,
        cookie: { maxAge: 1000 * 60 * 60 * 24 },
      })
    );
    ```

### Authentication

- **Local Strategy**: Users can log in with their email and password. Passport's `passport-local` strategy is used to authenticate users.
  
    ```javascript
    passport.use(
      "local",
      new Strategy(async function verify(username, password, cb) {
        // Authentication logic
      })
    );
    ```

### Google OAuth2 Integration

- **Google Strategy**: Users can log in using their Google account. Passport's `passport-google-oauth2` strategy is configured with client ID, client secret, and callback URL.

    ```javascript
    passport.use(
      "google",
      new GoogleStrategy(
        {
          clientID: process.env.GOOGLE_CLIENT_ID,
          clientSecret: process.env.GOOGLE_CLIENT_SECRET,
          callbackURL: "http://localhost:3000/auth/google/page",
          userProfileURL: "https://www.googleapis.com/oauth2/v3/userinfo",
        },
        async (accessToken, refreshToken, profile, cb) => {
          // Google authentication logic
        }
      )
    );
    ```

### Password Hashing

- **bcrypt**: Passwords are hashed using `bcrypt` before being stored in the database, ensuring that even if the database is compromised, passwords remain secure.

    ```javascript
    bcrypt.hash(password, saltRounds, async (err, hash) => {
      // Hashing logic
    });
    ```

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.
# AuthNest
