
Setting up a complete authentication and authorization flow with user login, JWT, and OAuth involves integrating multiple steps and components. Here’s a step-by-step guide to implementing the system using **Node.js**, **JWT** for stateless authentication, and **OAuth** (e.g., Google) for third-party login.

### Components Overview:

1. **User Registration** and **Login** using JWT for regular user credentials.
2. **OAuth** integration for third-party authentication (e.g., Google).
3. **Role-Based Access Control (RBAC)** or authorization middleware.
4. Secure the API endpoints and ensure only authenticated users can access them.

### Tech Stack:

- **Node.js** (backend server).
- **Express** (web framework).
- **jsonwebtoken** (for JWT generation and verification).
- **bcryptjs** (for hashing passwords).
- **passport** (for OAuth, using `passport-google-oauth20`).
- **mongoose** (for MongoDB, or use any other DB).
- **express-validator** (for validating inputs).
- **dotenv** (for managing environment variables).

### Step 1: Project Setup

Install the required dependencies:
```bash
npm init -y
npm install express mongoose bcryptjs jsonwebtoken passport passport-google-oauth20 express-session dotenv
```

Set up `.env` for sensitive information such as JWT secret and OAuth credentials:
```bash
PORT=3000 
JWT_SECRET=your_jwt_secret_key 
GOOGLE_CLIENT_ID=your_google_client_id GOOGLE_CLIENT_SECRET=your_google_client_secret 
MONGO_URI=your_mongodb_uri
```

### Step 2: User Registration and JWT-Based Login

#### User Model (MongoDB Example)
```js
// models/User.js
const mongoose = require('mongoose');
const bcrypt = require('bcryptjs');

const UserSchema = new mongoose.Schema({
  username: { type: String, required: true, unique: true },
  email: { type: String, required: true, unique: true },
  password: { type: String, required: true },
  googleId: { type: String }, // For Google OAuth users
  role: { type: String, default: 'user' }
});

// Hash the password before saving
UserSchema.pre('save', async function (next) {
  if (!this.isModified('password')) return next();
  const salt = await bcrypt.genSalt(10);
  this.password = await bcrypt.hash(this.password, salt);
  next();
});

// Match password method
UserSchema.methods.matchPassword = async function (enteredPassword) {
  return await bcrypt.compare(enteredPassword, this.password);
};

module.exports = mongoose.model('User', UserSchema);
```

#### User Registration & JWT Login

```js
// routes/auth.js
const express = require('express');
const bcrypt = require('bcryptjs');
const jwt = require('jsonwebtoken');
const { body, validationResult } = require('express-validator');
const User = require('../models/User');
const router = express.Router();

// JWT Secret
const JWT_SECRET = process.env.JWT_SECRET;

// Register a new user
router.post('/register', [
  body('username').notEmpty(),
  body('email').isEmail(),
  body('password').isLength({ min: 6 })
], async (req, res) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) return res.status(400).json({ errors: errors.array() });

  const { username, email, password } = req.body;

  try {
    let user = await User.findOne({ email });
    if (user) return res.status(400).json({ msg: 'User already exists' });

    user = new User({ username, email, password });
    await user.save();

    const payload = { userId: user._id, role: user.role };
    const token = jwt.sign(payload, JWT_SECRET, { expiresIn: '1h' });

    res.json({ token });
  } catch (err) {
    console.error(err.message);
    res.status(500).send('Server error');
  }
});

// Login a user
router.post('/login', [
  body('email').isEmail(),
  body('password').exists()
], async (req, res) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) return res.status(400).json({ errors: errors.array() });

  const { email, password } = req.body;

  try {
    let user = await User.findOne({ email });
    if (!user) return res.status(400).json({ msg: 'Invalid credentials' });

    const isMatch = await user.matchPassword(password);
    if (!isMatch) return res.status(400).json({ msg: 'Invalid credentials' });

    const payload = { userId: user._id, role: user.role };
    const token = jwt.sign(payload, JWT_SECRET, { expiresIn: '1h' });

    res.json({ token });
  } catch (err) {
    console.error(err.message);
    res.status(500).send('Server error');
  }
});

module.exports = router;

```

  

### Step 3: Google OAuth Setup
#### Passport Google OAuth Strategy
```js
// config/passport.js
const passport = require('passport');
const GoogleStrategy = require('passport-google-oauth20').Strategy;
const User = require('../models/User');

passport.use(new GoogleStrategy({
  clientID: process.env.GOOGLE_CLIENT_ID,
  clientSecret: process.env.GOOGLE_CLIENT_SECRET,
  callbackURL: '/auth/google/callback'
}, async (accessToken, refreshToken, profile, done) => {
  try {
    let user = await User.findOne({ googleId: profile.id });

    if (!user) {
      user = new User({ googleId: profile.id, username: profile.displayName, email: profile.emails[0].value });
      await user.save();
    }

    done(null, user);
  } catch (err) {
    done(err, null);
  }
}));

passport.serializeUser((user, done) => done(null, user.id));
passport.deserializeUser((id, done) => {
  User.findById(id, (err, user) => done(err, user));
});


```

#### Google OAuth Routes
```js
// routes/oauth.js
const express = require('express');
const passport = require('passport');
const jwt = require('jsonwebtoken');
const router = express.Router();

// Google OAuth route
router.get('/google', passport.authenticate('google', { scope: ['profile', 'email'] }));

// Google OAuth callback
router.get('/google/callback', passport.authenticate('google', { failureRedirect: '/' }), (req, res) => {
  const payload = { userId: req.user._id, role: req.user.role };
  const token = jwt.sign(payload, process.env.JWT_SECRET, { expiresIn: '1h' });

  res.redirect(`/profile?token=${token}`);
});

module.exports = router;
```

### Step 4: Authorization Middleware
You’ll need a middleware that checks the validity of the JWT and controls access based on user roles.

```js
// middleware/auth.js
const jwt = require('jsonwebtoken');

const authMiddleware = (roles = []) => {
  return (req, res, next) => {
    const token = req.headers['authorization'];

    if (!token) return res.status(403).send('Token is required');

    try {
      const decoded = jwt.verify(token, process.env.JWT_SECRET);
      req.user = decoded;

      if (roles.length && !roles.includes(decoded.role)) {
        return res.status(401).send('Unauthorized');
      }

      next();
    } catch (err) {
      res.status(401).send('Invalid Token');
    }
  };
};

module.exports = authMiddleware;

```

  

### Step 5: Protected Routes

Use the `authMiddleware` to protect your routes based on the role of the user:

```js
// routes/protected.js
const express = require('express');
const authMiddleware = require('../middleware/auth');
const router = express.Router();

// Example: Admin-only route
router.get('/admin', authMiddleware(['admin']), (req, res) => {
  res.send('Admin content');
});

// Example: Open to any authenticated user
router.get('/dashboard', authMiddleware(), (req, res) => {
  res.send('Dashboard content');
});

module.exports = router;

```

### Step 6: Complete Express Setup

```js
// app.js
const express = require('express');
const mongoose = require('mongoose');
const passport = require('passport');
const session = require('express-session');
require('dotenv').config();
require('./config/passport'); // Passport config

const app = express();

// Connect to MongoDB
mongoose.connect(process.env.MONGO_URI, { useNewUrlParser: true, useUnifiedTopology: true });

// Middleware
app.use(express.json());
app.use(session({ secret: 'session-secret', resave: false, saveUninitialized: true }));
app.use(passport.initialize());
app.use(passport.session());

// Routes
app.use('/auth', require('./routes/auth'));
app.use('/auth/google', require('./routes/oauth'));
app.use('/protected', require('./routes/protected'));

app.listen(process.env.PORT, () => {
  console.log(`Server running on port ${process.env.PORT}`);
});


```


### Summary

- **User Login/Registration**: Using JWT for stateless authentication.

- **OAuth (Google)**: Allows users to log in using their Google account.

- **Authorization**: Role-based middleware that checks if users have access to certain routes.

- **Protected Routes**: Only accessible to authenticated users, with optional role-based restrictions.This is a robust authentication and authorization flow using JWT and OAuth for Node.js. Let me know if you need any further clarifications or improvements!