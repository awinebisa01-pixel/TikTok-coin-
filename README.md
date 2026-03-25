
const express = require('express');
const app = express();

app.use(express.json());
app.use(express.static('public'));

let users = [];

app.post('/signup', (req, res) => {
  const {username, password} = req.body;
  users.push({username, password, coins: 0});
  res.json({message: "Account created"});
});

app.post('/login', (req, res) => {
  const {username, password} = req.body;
  const user = users.find(u => u.username === username && u.password === password);

  if(user){
    res.json({message: "Login successful", coins: user.coins});
  } else {
    res.json({message: "Invalid login"});
  }
});

app.post('/pay', (req, res) => {
  const {username, coins} = req.body;
  const user = users.find(u => u.username === username);

  if(user){
    user.coins += coins;
    res.json({message: "Coins added 🎉", total: user.coins});
  }
});

app.listen(3000);