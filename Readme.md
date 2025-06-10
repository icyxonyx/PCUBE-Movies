**PCUBE Movies** 🎥
A full-stack movie booking application where users can browse movies, view theatre showtimes, and book tickets with Stripe payment integration. Built with Node.js, Express, MongoDB, and React/Redux.

---

## Table of Contents

1. [Features](#features)
2. [Technologies Used](#technologies-used)
3. [Getting Started](#getting-started)

   * [Prerequisites](#prerequisites)
   * [Installation](#installation)
   * [Environment Variables](#environment-variables)
   * [Running in Development](#running-in-development)
   * [Building for Production](#building-for-production)
4. [Project Structure](#project-structure)
5. [Backend API Endpoints](#backend-api-endpoints)

   * [User Authentication](#user-authentication)
   * [Movie Management](#movie-management)
   * [Theatre & Show Management](#theatre--show-management)
   * [Booking & Stripe Payment](#booking--stripe-payment)
6. [Frontend Overview](#frontend-overview)
7. [License](#license)

---

## Features

* **User Authentication**

  * Register with name, email, and password
  * Login with JWT-based authentication
  * Middleware protects private routes

* **Movie Catalog**

  * Admin or authenticated users can add, update, and delete movies
  * Retrieve all movies or fetch a single movie by ID

* **Theatre & Show Management**

  * Add new theatres (name, address, phone, seats configuration, owner ID)
  * Update or delete existing theatres
  * Add shows to a theatre (movie, date, time, available seats)
  * Fetch all theatres, theatres by owner, or theatres showing a particular movie
  * Retrieve show details by ID

* **Booking & Payments (Stripe)**

  * Book a show by selecting seats
  * Stripe integration for secure payment processing
  * Create a Booking record (linked to user, show, seats, and transaction ID)
  * Fetch all bookings for the logged-in user

* **Responsive React Frontend**

  * React + Redux application for browsing movies and theatres
  * Pages for Login, Registration, Movie Listings, Theatre Listings, Show Listings
  * Book a show via a multi-step flow: select theatre → select showtime → choose seats → pay with Stripe
  * User profile page displaying past bookings and, if applicable, theatre management for theatre owners

---

## Technologies Used

**Backend (Node.js + Express + MongoDB)**

* Node.js v18.x
* Express v4.x
* MongoDB with Mongoose v8.x
* JSON Web Tokens (jsonwebtoken) for auth
* bcryptjs for password hashing
* dotenv for environment variable management
* Stripe (`stripe` npm package) for payment processing
* Nodemon for development
* CORS middleware

**Frontend (React + Redux)**

* React v18.x (Create React App)
* Redux Toolkit (Redux + Redux Thunk)
* React Router v6 for client-side routing
* Axios for HTTP requests
* react-stripe-checkout for Stripe UI component
* CSS modules and custom stylesheets (`.css`)

---

## Getting Started

### Prerequisites

1. **Node.js** (v14 or later)
2. **npm** (v6 or later) OR **yarn**
3. **MongoDB** (local installation or Atlas cluster)
4. **Stripe Account** (for payment processing; obtain a test or live secret key)

---

### Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/icyxonyx/PCUBE-Movies.git
   cd "PCUBE Movies"
   ```

2. **Install Backend Dependencies**

   ```bash
   cd server
   npm install
   ```

3. **Install Frontend Dependencies**

   ```bash
   cd ../client
   npm install
   ```

---

### Environment Variables

Create a `.env` file inside the `server` directory with the following keys (replace placeholder values):

```
mongo_url=<YOUR_MONGODB_CONNECTION_STRING>
jwt_secret=<YOUR_JWT_SECRET>
stripe_key=<YOUR_STRIPE_SECRET_KEY>
```

* **mongo\_url** – MongoDB connection URI (e.g., `mongodb+srv://username:password@cluster0.mongodb.net/dbname`).
* **jwt\_secret** – Secret used to sign and verify JWTs.
* **stripe\_key** – Your Stripe Secret Key (for test or live mode).

You can rename `.env.example` to `.env` and fill in your values:

```
cp server/.env.example server/.env
```

---

### Running in Development

1. **Start the Backend Server**

   ```bash
   cd server
   npm run dev
   ```

   * Listens on **PORT** (defaults to `5000` if not set).
   * Auto-restarts on file changes with Nodemon.

2. **Start the Frontend Client**

   ```bash
   cd ../client
   npm start
   ```

   * Opens React app at [http://localhost:3000](http://localhost:3000).
   * Proxy is configured so API calls to `/api/**` route to `localhost:5000`.

You should now be able to register, log in, and exercise all features from the React UI.

---

### Building for Production

1. **Build the React App**

   ```bash
   cd client
   npm run build
   ```

   * Generates optimized static assets in `client/build`.

2. **Serve the React Build from Express**

   * Ensure NODE\_ENV is set to `production` and start the server:

     ```bash
     cd ../server
     NODE_ENV=production node server.js
     ```
   * The Express server automatically serves files from `server/client/build` under production.
   * The API remains available under `/api/**`.

---

## Project Structure

```
PCUBE Movies/
├── .env                 # Environment variables for the server
├── .gitignore
├── nodemon.json         # Nodemon configuration for development
├── package.json         # Root (unused) or documentation—actual server package.json is in server/
├── README.md            # ← This file
│
├── server/              # Backend source code
│   ├── .env.example     # Example environment variables
│   ├── package.json     # Backend dependencies & scripts
│   ├── server.js        # Entry point for Express server
│   ├── config/
│   │   └── dbConfig.js  # MongoDB connection setup
│   ├── middlewares/
│   │   └── authMiddleware.js  # JWT authentication checker
│   ├── models/          # Mongoose models
│   │   ├── userModel.js
│   │   ├── movieModel.js
│   │   ├── theatreModel.js
│   │   ├── showModel.js
│   │   └── bookingModel.js
│   ├── routes/          # Express route handlers
│   │   ├── usersRoute.js
│   │   ├── moviesRoute.js
│   │   ├── theatresRoute.js
│   │   └── bookingsRoute.js
│   └── package-lock.json
│
└── client/              # React front-end (Create React App)
    ├── public/
    │   ├── index.html
    │   ├── favicon.ico
    │   └── manifest.json
    ├── src/
    │   ├── components/      # Reusable UI components
    │   ├── pages/           # Top-level pages (Login, Register, Movies, Theatres, Book, Profile)
    │   ├── redux/
    │   │   ├── store.js     # Redux store configuration
    │   │   ├── usersSlice.js
    │   │   ├── moviesSlice.js
    │   │   ├── theatresSlice.js
    │   │   └── bookingsSlice.js
    │   ├── stylesheets/     # Custom CSS files
    │   ├── App.js           # Main App component with React Router
    │   ├── index.js         # ReactDOM render + Redux Provider
    │   └── utils/           # Helper functions (e.g. setAuthToken)
    ├── package.json
    └── package-lock.json
```

---

## Backend API Endpoints

> All routes under `server/routes/**` expect or return JSON. Protected routes require an `Authorization: Bearer <token>` header.

### User Authentication

* **POST /api/users/register**
  Register a new user.
  **Body:**

  ```json
  {
    "name": "Alice",
    "email": "alice@example.com",
    "password": "SecurePass123"
  }
  ```

  **Response:**

  ```json
  { "success": true, "message": "User registered successfully" }
  ```

  Errors if email already exists or missing fields.

* **POST /api/users/login**
  Log in an existing user.
  **Body:**

  ```json
  {
    "email": "alice@example.com",
    "password": "SecurePass123"
  }
  ```

  **Response (on success):**

  ```json
  {
    "success": true,
    "token": "Bearer <jwt_token>",
    "data": {
      "_id": "...",
      "name": "Alice",
      "email": "alice@example.com",
      "isAdmin": false
    }
  }
  ```

  Returns a JWT and user info.

* **GET /api/users/get-user-info-by-id/\:userId** *(Protected)*
  Fetch user details by ID.
  **Headers:** `Authorization: Bearer <token>`
  **Response:**

  ```json
  {
    "success": true,
    "data": {
      "_id": "...",
      "name": "Alice",
      "email": "alice@example.com",
      "isAdmin": false
    }
  }
  ```

---

### Movie Management

* **POST /api/movies/add-movie** *(Protected)*
  Add a new movie.
  **Body:**

  ```json
  {
    "title": "Inception",
    "description": "A mind-bending thriller...",
    "duration": 148,
    "language": "English",
    "genre": "Sci-Fi",
    "cast": ["Leonardo DiCaprio", "Joseph Gordon-Levitt"],
    "releaseDate": "2010-07-16T00:00:00.000Z",
    "poster": "https://image.url/inception.jpg"
  }
  ```

  **Response:**

  ```json
  { "success": true, "message": "Movie added successfully" }
  ```

* **GET /api/movies/get-all-movies**
  Fetch all movies in the catalog.
  **Response:**

  ```json
  { "success": true, "data": [ { /* movie objects */ }, … ] }
  ```

* **POST /api/movies/update-movie** *(Protected)*
  Update an existing movie’s fields (provide entire object with `_id`).
  **Body:**

  ```json
  {
    "_id": "60a7b3e8...",
    "title": "Inception (2010)",
    "description": "A heist within dreams...",
    // other updated fields…
  }
  ```

  **Response:**

  ```json
  { "success": true, "message": "Movie updated successfully" }
  ```

* **POST /api/movies/delete-movie** *(Protected)*
  Delete a movie by its ID.
  **Body:**

  ```json
  { "movieId": "60a7b3e8..." }
  ```

  **Response:**

  ```json
  { "success": true, "message": "Movie deleted successfully" }
  ```

* **GET /api/movies/get-movie-by-id/\:id**
  Fetch details for one movie by its MongoDB `_id`.
  **Response:**

  ```json
  {
    "success": true,
    "data": { /* movie object */ }
  }
  ```

---

### Theatre & Show Management

* **POST /api/theatres/add-theatre** *(Protected)*
  Add a new theatre.
  **Body:**

  ```json
  {
    "name": "AMC Empire 25",
    "address": "234 W 42nd St, New York, NY",
    "phone": "+1-212-398-1200",
    "numberOfSeats": 300,
    "owner": "<userId>",
    "isActive": true
  }
  ```

  **Response:**

  ```json
  { "success": true, "message": "Theatre added successfully" }
  ```

* **GET /api/theatres/get-all-theatres** *(Protected)*
  Get a list of all active theatres.
  **Response:**

  ```json
  { "success": true, "data": [ { /* theatre objects */ }, … ] }
  ```

* **POST /api/theatres/get-all-theatres-by-owner** *(Protected)*
  Fetch theatres created by a specific owner.
  **Body:**

  ```json
  { "ownerId": "<userId>" }
  ```

  **Response:**

  ```json
  { "success": true, "data": [ /* theatres for that owner */ ] }
  ```

* **POST /api/theatres/update-theatre** *(Protected)*
  Update a theatre’s info.
  **Body:**

  ```json
  {
    "_id": "60b8c4f2...",
    "name": "AMC Empire 25 (Updated)",
    "address": "…",
    // other fields…
  }
  ```

  **Response:**

  ```json
  { "success": true, "message": "Theatre updated successfully" }
  ```

* **POST /api/theatres/delete-theatre** *(Protected)*
  Delete a theatre by ID.
  **Body:**

  ```json
  { "theatreId": "60b8c4f2..." }
  ```

  **Response:**

  ```json
  { "success": true, "message": "Theatre deleted successfully" }
  ```

* **POST /api/theatres/add-show** *(Protected)*
  Add a new showtime to an existing theatre.
  **Body:**

  ```json
  {
    "name": "Inception - 7:00 PM",
    "date": "2025-06-15T00:00:00.000Z",
    "time": "19:00",
    "movie": "<movieId>",
    "seats": [
      "A1", "A2", "A3", // initial available seats
      …
    ],
    "theatre": "<theatreId>"
  }
  ```

  **Response:**

  ```json
  { "success": true, "message": "Show added successfully" }
  ```

* **POST /api/theatres/get-all-shows-by-theatre** *(Protected)*
  Get all shows for a specific theatre.
  **Body:**

  ```json
  { "theatreId": "<theatreId>" }
  ```

  **Response:**

  ```json
  { "success": true, "data": [ /* show objects */ ] }
  ```

* **POST /api/theatres/delete-show** *(Protected)*
  Delete a show by ID.
  **Body:**

  ```json
  { "showId": "60c9d5a4..." }
  ```

  **Response:**

  ```json
  { "success": true, "message": "Show deleted successfully" }
  ```

* **POST /api/theatres/get-all-theatres-by-movie** *(Protected)*
  Find all theatres currently showing a particular movie.
  **Body:**

  ```json
  { "movieId": "<movieId>" }
  ```

  **Response:**

  ```json
  { "success": true, "data": [ /* theatre objects */ ] }
  ```

* **POST /api/theatres/get-show-by-id** *(Protected)*
  Retrieve a single show’s details (including available seats).
  **Body:**

  ```json
  { "showId": "<showId>" }
  ```

  **Response:**

  ```json
  { "success": true, "data": { /* show object */ } }
  ```

---

### Booking & Stripe Payment

* **POST /api/bookings/make-payment** *(Protected)*
  Process payment and create a booking record.
  **Body:**

  ```json
  {
    "token": { /* Stripe token object from frontend */ },
    "amount": 2000,          // amount in cents (e.g., $20.00 = 2000)
    "showId": "<showId>",
    "userId": "<userId>",
    "seats": ["A1", "A2", "A3"]
  }
  ```

  **Flow:**

  1. Create a Stripe customer with `token.email` and `token.id`.
  2. Charge the customer for `amount`.
  3. On successful charge, create a Booking in MongoDB:

     ```json
     {
       "show": "<showId>",
       "user": "<userId>",
       "seats": ["A1", "A2", "A3"],
       "transactionId": "<stripeCharge.id>"
     }
     ```
  4. Update the Show’s `bookedSeats` array to include purchased seats.
     **Response (on success):**

  ```json
  { "success": true, "message": "Booking successful!" }
  ```

  If payment or DB operation fails, returns `{ success: false, message: <error> }`.

* **POST /api/bookings/get-bookings-by-user-id** *(Protected)*
  Retrieve all past bookings for the logged-in user.
  **Body:**

  ```json
  { "userId": "<userId>" }
  ```

  **Response:**

  ```json
  {
    "success": true,
    "data": [
      {
        "_id": "60d1e7f9...",
        "show": { /* nested show details with movie & theatre populated */ },
        "seats": ["A1","A2"],
        "transactionId": "ch_1J0ZpDLi...",
        "createdAt": "2025-06-01T14:23:05.123Z",
        …
      },
      …
    ]
  }
  ```

---

## Frontend Overview

The React client (located in `client/`) is a single-page application created with Create React App.

### Key Concepts

1. **Authentication Flow**

   * **Register Page** (`/register`) and **Login Page** (`/login`)
   * After successful login, JWT token is stored in `localStorage` and axios is configured to include it in the `Authorization` header.
   * Protected routes use a custom `PrivateRoute` component that checks for a valid token and redirects to `/login` if missing/expired.

2. **Redux State Management**

   * **store.js** configures Redux Toolkit store, combining these slices:

     * `usersSlice.js` handles authentication state (user info & token).
     * `moviesSlice.js` fetches and stores movie list for browsing.
     * `theatresSlice.js` fetches theatres and showtimes.
     * `bookingsSlice.js` tracks current booking and past bookings.
   * Each slice contains async thunk actions (`createAsyncThunk`) for API calls, plus reducers for pending/success/error.

3. **Router & Pages**

   * **`<App />`** (top-level) defines routes with React Router v6:

     * `/` → **Home Page**: Featured movies carousel or list.
     * `/login` → **LoginPage.jsx**
     * `/register` → **RegisterPage.jsx**
     * `/movies` → **MoviesList.jsx**: Browse all movies; click a movie to see details.
     * `/movies/:movieId` → **MovieDetails.jsx**: Show description, poster, “Find Theatres” button.
     * `/theatres/:movieId` → **TheatresForMovie.jsx**: List of theatres playing that movie.
     * `/shows/:showId` → **ShowDetails.jsx**: Show date/time, available seats grid to select.
     * `/book/:showId` → **BookingPage.jsx**: Select seats (checkbox grid), then Stripe checkout.
     * `/profile` → **Profile.jsx**: Display user info, past bookings, and (if `isAdmin` or theatre owner) management links.
     * `/profile/theatres` → **TheatresList.jsx**: A theatre owner can view, update, or delete their theatres.
     * `/profile/theatres/add` → **TheatreForm.jsx**: Add or edit theatre details.
     * `/profile/theatres/:theatreId/shows` → **ShowsList.jsx**: List & manage shows in that theatre.
     * `/profile/bookings` → **Bookings.jsx**: List of past bookings for the logged-in user.

4. **Components & Styles**

   * Common UI components in `src/components/` (e.g. `Navbar.jsx`, `Footer.jsx`, `MovieCard.jsx`, `TheatreCard.jsx`, `ShowCard.jsx`, `SeatSelector.jsx`).
   * Custom CSS in `src/stylesheets/` for layout, form elements, and basic responsive design.
   * Uses simple CSS classes (no CSS-in-JS) plus a minimal reliance on third-party UI libraries (mostly custom).

5. **Stripe Integration**

   * On the **BookingPage**, user selects seats from the `Show`'s `seats` array.
   * The component collects a Stripe token using `react-stripe-checkout`.
   * On “Pay” submission, an Axios POST is made to `/api/bookings/make-payment` with `{ token, amount, showId, userId, seats }`.
   * On success, the UI displays a confirmation message and redirects to `/profile/bookings`.

---

## License

This project is licensed under the MIT License. Feel free to fork, modify, and use as needed.
