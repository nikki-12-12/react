import React from 'react';
import ReactDOM from 'react-dom';
import { BrowserRouter as Router } from 'react-router-dom';
import App from './App';

ReactDOM.render(
  <React.StrictMode>
    <Router>
      <App />
    </Router>
  </React.StrictMode>,
  document.getElementById('root')
);

app.js
import React from 'react';
import { Routes, Route } from 'react-router-dom';
import Navbar from './components/Navbar';
import PrivateRoute from './components/PrivateRoute';
import Home from './pages/Home';
import About from './pages/About';
import Login from './pages/Login';
import Dashboard from './pages/Dashboard';
import Profile from './pages/Profile';
import Products from './pages/Products';
import Product from './pages/Product';

function App() {
  const [isAuthenticated, setIsAuthenticated] = React.useState(false);

  return (
    <div className="app">
      <Navbar isAuthenticated={isAuthenticated} />
      
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/login" element={<Login setIsAuthenticated={setIsAuthenticated} />} />
        <Route path="/products" element={<Products />} />
        <Route path="/products/:id" element={<Product />} />
        
        {/* Rutas protegidas */}
        <Route element={<PrivateRoute isAuthenticated={isAuthenticated} />}>
          <Route path="/dashboard" element={<Dashboard />} />
          <Route path="/profile" element={<Profile />} />
        </Route>
      </Routes>
    </div>
  );
}

export default App;


navbar.jsx
import React from 'react';
import { NavLink } from 'react-router-dom';

const Navbar = ({ isAuthenticated }) => {
  // Estilo para los enlaces activos
  const activeStyle = {
    fontWeight: 'bold',
    color: '#4CAF50',
    textDecoration: 'underline'
  };

  return (
    <nav className="navbar">
      <ul className="nav-list">
        <li className="nav-item">
          <NavLink 
            to="/" 
            style={({ isActive }) => isActive ? activeStyle : undefined}
            className="nav-link"
          >
            Inicio
          </NavLink>
        </li>
        <li className="nav-item">
          <NavLink 
            to="/about" 
            style={({ isActive }) => isActive ? activeStyle : undefined}
            className="nav-link"
          >
            Acerca de
          </NavLink>
        </li>
        <li className="nav-item">
          <NavLink 
            to="/products" 
            style={({ isActive }) => isActive ? activeStyle : undefined}
            className="nav-link"
          >
            Productos
          </NavLink>
        </li>
        
        {isAuthenticated ? (
          <>
            <li className="nav-item">
              <NavLink 
                to="/dashboard" 
                style={({ isActive }) => isActive ? activeStyle : undefined}
                className="nav-link"
              >
                Dashboard
              </NavLink>
            </li>
            <li className="nav-item">
              <NavLink 
                to="/profile" 
                style={({ isActive }) => isActive ? activeStyle : undefined}
                className="nav-link"
              >
                Perfil
              </NavLink>
            </li>
          </>
        ) : (
          <li className="nav-item">
            <NavLink 
              to="/login" 
              style={({ isActive }) => isActive ? activeStyle : undefined}
              className="nav-link"
            >
              Login
            </NavLink>
          </li>
        )}
      </ul>
    </nav>
  );
};

export default Navbar;

privatetoute.jsx
import React from 'react';
import { Navigate, Outlet } from 'react-router-dom';

const PrivateRoute = ({ isAuthenticated }) => {
  return isAuthenticated ? <Outlet /> : <Navigate to="/login" />;
};

export default PrivateRoute;

home.jsx
import React from 'react';

const Home = () => {
  return (
    <div className="page home">
      <h1>Bienvenido a nuestra aplicación</h1>
      <p>Esta es la página de inicio. Navega usando el menú superior.</p>
    </div>
  );
};

export default Home;

