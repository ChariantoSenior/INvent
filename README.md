RAdar/
├── client/
│   ├── public/
│   │   └── index.html
│   ├── src/
│   │   ├── components/
│   │   │   └── (Component files)
│   │   ├── pages/
│   │   │   └── (Page files)
│   │   ├── services/
│   │   │   └── authService.js
│   │   ├── App.js
│   │   ├── index.js
│   │   └── App.css
├── server/
│   ├── controllers/
│   │   ├── authController.js
│   ├── models/
│   │   ├── User.js
│   ├── routes/
│   │   ├── auth.js
│   ├── middlewares/
│   │   └── (Middleware files)
│   ├── config/
│   │   └── (Configuration files)
│   ├── server.js
├── .env
├── .gitignore
├── package.json
├── client/package.json
├── README.md
cd path/to/RAdar
git init
cd server
npm init -y
npm install express mongoose bcryptjs jsonwebtoken dotenv cors
// server/server.js
const express = require('express');
const mongoose = require('mongoose');
const cors = require('cors');
const path = require('path');
const dotenv = require('dotenv');

dotenv.config();

const app = express();
app.use(cors());
app.use(express.json());

const authRoutes = require('./routes/auth');

app.use('/api/auth', authRoutes);

if (process.env.NODE_ENV === 'production') {
  app.use(express.static(path.join(__dirname, '../client/build')));
  app.get('*', (req, res) => {
    res.sendFile(path.join(__dirname, '../client/build', 'index.html'));
  });
}

const PORT = process.env.PORT || 5000;
const MONGO_URI = process.env.MONGO_URI;

mongoose.connect(MONGO_URI, { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => {
    app.listen(PORT, () => {
      console.log(`Server running on port ${PORT}`);
    });
  })
  .catch(err => console.error('Error connecting to MongoDB:', err));
