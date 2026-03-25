<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bigman Holdings System v1.0</title>
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <style>
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; margin: 0; background-color: #f4f7f6; color: #333; }
        .login-container { height: 100vh; display: flex; justify-content: center; alignItems: center; background: linear-gradient(135deg, #1a3a5f 0%, #2c3e50 100%); }
        .login-box { background: white; padding: 40px; border-radius: 15px; box-shadow: 0 10px 25px rgba(0,0,0,0.2); text-align: center; width: 320px; }
        .nav-sidebar { width: 240px; background: #1a3a5f; color: white; height: 100vh; position: fixed; padding: 20px; }
        .main-content { margin-left: 280px; padding: 30px; }
        .card { background: white; padding: 20px; border-radius: 12px; box-shadow: 0 4px 6px rgba(0,0,0,0.05); margin-bottom: 20px; }
        .btn-primary { background: #1a3a5f; color: white; border: none; padding: 10px 20px; border-radius: 6px; cursor: pointer; font-weight: bold; }
        .status-pill { padding: 4px 10px; border-radius: 20px; font-size: 12px; color: white; }
        table { width: 100%; border-collapse: collapse; margin-top: 15px; }
        th { text-align: left; border-bottom: 2px solid #eee; padding: 10px; }
        td { padding: 10px; border-bottom: 1px solid #f0f0f0; }
        .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 20px; }
    </style>
</head>
<body>
    <div id="root"></div>

    <script type="text/babel">
        const { useState, useEffect } = React;

        function App() {
            const [isLoggedIn, setIsLoggedIn] = useState(false);
            const [currentPage, setCurrentPage] = useState('dashboard');
            const [pin, setPin] = useState('');
            const [error, setError] = useState('');

            // 1. LOGIN HANDLER
            const handleLogin = (e) => {
                e.preventDefault();
                if (pin === '1234') {
                    setIsLoggedIn(true);
                } else {
                    setError('❌ Access Denied');
                    setPin('');
                }
            };

            if (!isLoggedIn) {
                return (
                    <div className="login-container">
                        <div className="login-box">
                            <h2 style={{color: '#1a3a5f'}}>BIGMAN HOLDINGS</h2>
                            <p>Enter Admin PIN</p>
                            <form onSubmit={handleLogin}>
                                <input 
                                    type="password" 
                                    value={pin} 
                                    onChange={(e) => setPin(e.target.value)} 
                                    style={{width: '80%', padding: '10px', fontSize: '20px', textAlign: 'center', marginBottom: '20px'}}
                                    maxLength="4"
                                />
                                <br/>
                                <button className="btn-primary" style={{width: '88%'}}>Unlock System</button>
                            </form>
                            {error && <p style={{color: 'red'}}>{error}</p>}
                        </div>
                    </div>
                );
            }

            return (
                <div>
                    <div className="nav-sidebar">
                        <h2>BH System</h2>
                        <hr/>
                        <p onClick={() => setCurrentPage('dashboard')} style={{cursor: 'pointer'}}>📊 Dashboard</p>
                        <p onClick={() => setCurrentPage('realestate')} style={{cursor: 'pointer'}}>🏠 Real Estate</p>
                        <p onClick={() => setCurrentPage('leads')} style={{cursor: 'pointer'}}>👥 Leads & CRM</p>
                        <p onClick={() => setIsLoggedIn(false)} style={{marginTop: '50px', color: '#ff7675', cursor: 'pointer'}}>🔒 Logout</p>
                    </div>

                    <div className="main-content">
                        {currentPage === 'dashboard' && <DashboardView />}
                        {currentPage === 'realestate' && <RealEstateView />}
                        {currentPage === 'leads' && <LeadsView />}
                    </div>
                </div>
            );
        }

        // 📊 DASHBOARD COMPONENT
        const DashboardView = () => (
            <div>
                <h1>Holdings Overview</h1>
                <div className="grid">
                    <div className="card" style={{background: '#1a3a5f', color: 'white'}}>
                        <h3>Total Monthly Revenue</h3>
                        <h2>TZS 45,250,000</h2>
                        <small>Across 5 Divisions</small>
                    </div>
                    <div className="card">
                        <h3>Overdue Rent</h3>
                        <h2 style={{color: '#e74c3c'}}>TZS 4,200,000</h2>
                        <small>8 Tenants Pending</small>
                    </div>
                </div>
                <div className="card">
                    <h3>WhatsApp Queue Status</h3>
                    <p>{navigator.onLine ? '✅ Internet Connected: Messages Syncing' : '📡 Offline Mode: Messages Saved in Queue'}</p>
                </div>
            </div>
        );

        // 🏠 REAL ESTATE COMPONENT
        const RealEstateView = () => (
            <div>
                <h1>Real Estate Division</h1>
                <div className="card">
                    <h3>Property Inventory</h3>
                    <table>
                        <thead>
                            <tr>
                                <th>Property</th>
                                <th>Status</th>
                                <th>Monthly Rent</th>
                                <th>Action</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr><td>Unit 4B - Sinza</td><td><span className="status-pill" style={{background: '#27ae60'}}>Occupied</span></td><td>450,000</td><td><button>Invoice</button></td></tr>
                            <tr><td>Unit 2A - Posta</td><td><span className="status-pill" style={{background: '#e74c3c'}}>Vacant</span></td><td>600,000</td><td><button>Add Tenant</button></td></tr>
                        </tbody>
                    </table>
                </div>
            </div>
        );

        // 👥 LEADS COMPONENT
        const LeadsView = () => (
            <div>
                <h1>Lead Management</h1>
                <div className="grid">
                    <div className="card">
                        <h4>Amina Hassan</h4>
                        <p>Interest: 4-Bedroom House</p>
                        <p>Budget: 200M TZS</p>
                        <span className="status-pill" style={{background: '#f39c12'}}>Qualified</span>
                    </div>
                    <div className="card">
                        <h4>Said Bakari</h4>
                        <p>Interest: Office Space</p>
                        <p>Budget: 1.2M Rent</p>
                        <span className="status-pill" style={{background: '#3498db'}}>New</span>
                    </div>
                </div>
            </div>
        );

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>
