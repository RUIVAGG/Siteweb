// DigitalHub - App Web com upload, player de vídeos e contas de jogos

import { BrowserRouter as Router, Route, Routes } from 'react-router-dom';
import { useState, useEffect } from 'react';
import axios from 'axios';

import { Home } from './pages/Home';
import { Login } from './pages/Login';
import { Register } from './pages/Register';
import { Dashboard } from './pages/Dashboard';
import { Upload } from './pages/Upload';
import { Videos } from './pages/Videos';
import { GameAccounts } from './pages/GameAccounts';
import { Navbar } from './components/Navbar';

export default function App() {
  const [user, setUser] = useState(null);
  const [token, setToken] = useState(localStorage.getItem('token'));

  useEffect(() => {
    if (token) {
      axios.get('/api/protected', {
        headers: { Authorization: `Bearer ${token}` },
      })
      .then(res => setUser(res.data.user))
      .catch(() => setUser(null));
    }
  }, [token]);

  return (
    <Router>
      <div className="min-h-screen bg-gray-100">
        <Navbar user={user} />
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/login" element={<Login setUser={setUser} setToken={setToken} />} />
          <Route path="/register" element={<Register />} />
          <Route path="/dashboard" element={<Dashboard user={user} token={token} />} />
          <Route path="/upload" element={<Upload token={token} />} />
          <Route path="/videos" element={<Videos token={token} />} />
          <Route path="/games" element={<GameAccounts token={token} />} />
        </Routes>
      </div>
    </Router>
  );
}
