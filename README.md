<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bigman Holdings Ltd | Unified ERP v2.5</title>
    <script src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <style>
        :root { --primary: #1a3a5f; --accent: #e67e22; --success: #27ae60; --danger: #c0392b; --bg: #f4f7f6; }
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; margin: 0; background: var(--bg); color: #333; }
        
        /* Sidebar & Layout */
        .sidebar { width: 260px; background: var(--primary); color: white; height: 100vh; position: fixed; padding: 20px; box-sizing: border-box; }
        .main-view { margin-left: 260px; padding: 40px; min-height: 100vh; box-sizing: border-box; }
        .nav-item { padding: 12px 15px; margin: 8px 0; border-radius: 8px; cursor: pointer; transition: 0.3s; display: flex; align-items: center; gap: 10px; }
        .nav-item:hover { background: rgba(255,255,255,0.1); }
        .active { background: var(--accent) !important; font-weight: bold; }

        /* UI Components */
        .card { background: white; padding: 25px; border-radius: 15px; box-shadow: 0 4px 12px rgba(0,0,0,0.05); margin-bottom: 25px; border: 1px solid #eee; }
        .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 20px; }
        .input-group { margin-bottom: 15px; }
        input, select { width: 100%; padding: 10px; margin-top: 5px; border: 1px solid #ddd; border-radius: 6px; box-sizing: border-box; }
        .btn { padding: 12px 20px; border: none; border-radius: 8px; cursor: pointer; font-weight: 600; transition: 0.2s; }
        .btn-primary { background: var(--primary); color: white; }
        .btn-success { background: var(--success); color: white; }
        
        /* Tables */
        table { width: 100%; border-collapse: collapse; margin-top: 15px; background: white; }
        th { text-align: left; padding: 15px; border-bottom: 2px solid var(--bg); background: #fafafa; font-size: 13px; text-transform: uppercase; }
        td { padding: 15px; border-bottom: 1px solid #eee; font-size: 14px; }

        /* Login */
        .login-overlay { position: fixed; inset: 0; background: var(--primary); display: flex; justify-content: center; align-items: center; z-index: 9999; }
        .login-card { background: white; padding: 40px; border-radius: 20px; text-align: center; width: 350px; }

        /* Images */
        .doc-preview { width: 100%; height: 120px; object-fit: cover; border-radius: 8px; margin-bottom: 10px; border: 1px solid #eee; }
    </style>
</head>
<body>
    <div id="root"></div>

    <script type="text/babel">
        const { useState, useEffect } = React;

        const App = () => {
            const [isLocked, setIsLocked] = useState(true);
            const [pin, setPin] = useState('');
            const [activeTab, setActiveTab] = useState('dashboard');

            // --- STATE DATA (LOCAL PERSISTENCE) ---
            const [employees, setEmployees] = useState(() => JSON.parse(localStorage.getItem('bh_staff')) || []);
            const [properties, setProperties] = useState(() => JSON.parse(localStorage.getItem('bh_props')) || []);
            const [docs, setDocs] = useState(() => JSON.parse(localStorage.getItem('bh_docs')) || []);

            // Auto-save whenever data changes
            useEffect(() => {
                localStorage.setItem('bh_staff', JSON.stringify(employees));
                localStorage.setItem('bh_props', JSON.stringify(properties));
                localStorage.setItem('bh_docs', JSON.stringify(docs));
            }, [employees, properties, docs]);

            // --- HANDLERS ---
            const addEmployee = (e) => {
                e.preventDefault();
                const form = e.target;
                const newEmp = {
                    id: Date.now(),
                    name: form.empName.value,
                    div: form.empDiv.value,
                    salary: form.empSalary.value,
                    date: new Date().toLocaleDateString()
                };
                setEmployees([...employees, newEmp]);
                form.reset();
            };

            const addProperty = (e) => {
                e.preventDefault();
                const form = e.target;
                const newProp = {
                    id: Date.now(),
                    title: form.pTitle.value,
                    tenant: form.pTenant.value || 'VACANT',
                    rent: form.pRent.value,
                    status: form.pStatus.value
                };
                setProperties([...properties, newProp]);
                form.reset();
            };

            const handleFileUpload = (e) => {
                const file = e.target.files[0];
                const reader = new FileReader();
                reader.onloadend = () => {
                    setDocs([...docs, { id: Date.now(), name: file.name, src: reader.result }]);
                };
                if (file) reader.readAsDataURL(file);
            };

            // --- LOGIN VIEW ---
            if (isLocked) {
                return (
                    <div className="login-overlay">
                        <div className="login-card">
                            <h1 style={{color: '#1a3a5f', marginBottom: '5px'}}>BIGMAN</h1>
                            <p style={{marginTop: 0, color: '#666'}}>HOLDINGS LTD ERP</p>
                            <hr style={{margin: '20px 0', opacity: 0.2}} />
                            <p>Enter 4-Digit Security PIN</p>
                            <input 
                                type="password" 
                                maxLength="4" 
                                value={pin} 
                                onChange={(e) => setPin(e.target.value)}
                                style={{fontSize: '32px', textAlign: 'center', letterSpacing: '10px', width: '200px', marginBottom: '20px'}}
                            />
                            <button 
                                className="btn btn-primary" 
                                style={{width: '100%', padding: '15px'}}
                                onClick={() => pin === '1234' ? setIsLocked(false) : alert('INVALID PIN')}
                            >UNLOCK ACCESS</button>
                        </div>
                    </div>
                );
            }

            return (
                <div style={{display: 'flex'}}>
                    {/* SIDEBAR */}
                    <div className="sidebar">
                        <h2 style={{borderBottom: '1px solid rgba(255,255,255,0.1)', paddingBottom: '15px'}}>BH System</h2>
                        <div className={`nav-item ${activeTab === 'dashboard' ? 'active' : ''}`} onClick={() => setActiveTab('dashboard')}>📊 Dashboard</div>
                        <div className={`nav-item ${activeTab === 'realestate' ? 'active' : ''}`} onClick={() => setActiveTab('realestate')}>🏠 Real Estate</div>
                        <div className={`nav-item ${activeTab === 'hr' ? 'active' : ''}`} onClick={() => setActiveTab('hr')}>👔 HR & Staff</div>
                        <div className={`nav-item ${activeTab['vault' ? 'active' : '']}`} onClick={() => setActiveTab('vault')}>📁 Document Vault</div>
                        <div style={{marginTop: '50px'}} className="nav-item" onClick={() => setIsLocked(true)}>🔒 Lock System</div>
                    </div>

                    {/* MAIN */}
                    <div className="main-view">
                        {/* 📊 DASHBOARD VIEW */}
                        {activeTab === 'dashboard' && (
                            <div>
                                <h1>Executive Summary</h1>
                                <div className="grid">
                                    <div className="card" style={{borderLeft: '6px solid var(--primary)'}}>
                                        <small>Registered Staff</small>
                                        <h2>{employees.length} Members</h2>
                                    </div>
                                    <div className="card" style={{borderLeft: '6px solid var(--accent)'}}>
                                        <small>Managed Units</small>
                                        <h2>{properties.length} Properties</h2>
                                    </div>
                                    <div className="card" style={{borderLeft: '6px solid var(--success)'}}>
                                        <small>Stored Documents</small>
                                        <h2>{docs.length} Files</h2>
                                    </div>
                                </div>
                                <div className="card">
                                    <h3>Division Overview</h3>
                                    <p>Welcome to the 2026 Bigman Holdings Management Interface. All data is encrypted and saved locally.</p>
                                </div>
                            </div>
                        )}

                        {/* 🏠 REAL ESTATE VIEW */}
                        {activeTab === 'realestate' && (
                            <div>
                                <h1>Real Estate Division</h1>
                                <div className="grid">
                                    <div className="card">
                                        <h3>Add New Property</h3>
                                        <form onSubmit={addProperty}>
                                            <input name="pTitle" placeholder="Property Name (e.g. Sinza Unit 4)" required />
                                            <input name="pTenant" placeholder="Tenant Name" />
                                            <input name="pRent" placeholder="Monthly Rent (TZS)" type="number" required />
                                            <select name="pStatus">
                                                <option value="Occupied">Occupied</option>
                                                <option value="Vacant">Vacant</option>
                                                <option value="Under Maintenance">Maintenance</option>
                                            </select>
                                            <button className="btn btn-success" style={{marginTop: '15px', width: '100%'}}>Save Property</button>
                                        </form>
                                    </div>
                                    <div className="card" style={{gridColumn: 'span 2'}}>
                                        <h3>Managed Inventory</h3>
                                        <table>
                                            <thead>
                                                <tr><th>Property</th><th>Tenant</th><th>Rent</th><th>Status</th></tr>
                                            </thead>
                                            <tbody>
                                                {properties.map(p => (
                                                    <tr key={p.id}>
                                                        <td>{p.title}</td>
                                                        <td>{p.tenant}</td>
                                                        <td>{Number(p.rent).toLocaleString()}</td>
                                                        <td style={{color: p.status === 'Vacant' ? 'red' : 'green'}}>{p.status}</td>
                                                    </tr>
                                                ))}
                                            </tbody>
                                        </table>
                                    </div>
                                </div>
                            </div>
                        )}

                        {/* 👔 HR VIEW */}
                        {activeTab === 'hr' && (
                            <div>
                                <h1>Staff & Payroll Management</h1>
                                <div className="grid">
                                    <div className="card">
                                        <h3>Register New Employee</h3>
                                        <form onSubmit={addEmployee}>
                                            <input name="empName" placeholder="Full Name" required />
                                            <select name="empDiv">
                                                <option>Real Estate</option>
                                                <option>Construction</option>
                                                <option>Car Dealership</option>
                                                <option>Electronics</option>
                                                <option>IT Solutions</option>
                                            </select>
                                            <input name="empSalary" placeholder="Base Salary (TZS)" type="number" required />
                                            <button className="btn btn-primary" style={{marginTop: '15px', width: '100%'}}>Enroll Staff</button>
                                        </form>
                                    </div>
                                    <div className="card" style={{gridColumn: 'span 2'}}>
                                        <h3>Employee Records</h3>
                                        <table>
                                            <thead>
                                                <tr><th>Name</th><th>Division</th><th>Salary</th><th>Joined</th></tr>
                                            </thead>
                                            <tbody>
                                                {employees.map(e => (
                                                    <tr key={e.id}>
                                                        <td><strong>{e.name}</strong></td>
                                                        <td>{e.div}</td>
                                                        <td>{Number(e.salary).toLocaleString()}</td>
                                                        <td>{e.date}</td>
                                                    </tr>
                                                ))}
                                            </tbody>
                                        </table>
                                    </div>
                                </div>
                            </div>
                        )}

                        {/* 📁 VAULT VIEW */}
                        {activeTab === 'vault' && (
                            <div>
                                <h1>Document Vault</h1>
                                <div className="card">
                                    <h3>Secure Upload</h3>
                                    <p>Upload scans of Title Deeds, TIN certificates, or Fundi receipts.</p>
                                    <input type="file" onChange={handleFileUpload} accept="image/*" />
                                </div>
                                <div className="grid">
                                    {docs.map(d => (
                                        <div key={d.id} className="card" style={{textAlign: 'center'}}>
                                            <img src={d.src} className="doc-preview" />
                                            <small>{d.name}</small>
                                            <br/>
                                            <button 
                                                style={{color: 'red', border: 'none', background: 'none', marginTop: '10px', cursor: 'pointer'}}
                                                onClick={() => setDocs(docs.filter(item => item.id !== d.id))}
                                            >Delete Permanent</button>
                                        </div>
                                    ))}
                                </div>
                            </div>
                        )}
                    </div>
                </div>
            );
        };

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>
