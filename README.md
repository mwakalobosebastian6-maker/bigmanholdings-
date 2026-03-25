<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bigman Holdings ERP - Stable v4.0</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    <style>
        :root {
            --primary: #2c3e50; --secondary: #3498db; --accent: #e74c3c; --light: #ecf0f1; --dark: #2c3e50; --success: #27ae60; --warning: #f39c12; --bg: #f4f7f6;
        }
        body { margin: 0; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background-color: var(--bg); color: #333; font-size: 14px; }
        
        /* Login */
        .login-container { display: flex; justify-content: center; align-items: center; height: 100vh; background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%); }
        .login-box { background: white; padding: 40px; border-radius: 10px; box-shadow: 0 10px 25px rgba(0,0,0,0.2); text-align: center; width: 350px; }
        input, select, textarea { width: 100%; padding: 10px; margin: 5px 0 15px; border: 1px solid #ddd; border-radius: 5px; box-sizing: border-box; font-family: inherit; }
        button { cursor: pointer; padding: 10px 15px; border: none; border-radius: 5px; font-weight: bold; transition: 0.2s; font-family: inherit; }
        .btn-login { background-color: var(--secondary); color: white; width: 100%; font-size: 16px; }
        .btn-primary { background-color: var(--primary); color: white; }
        .btn-success { background-color: var(--success); color: white; }
        .btn-warning { background-color: var(--warning); color: white; }
        .btn-danger { background-color: var(--accent); color: white; }
        .btn-outline { background: white; border: 1px solid #ccc; color: #666; }
        .error-msg { color: var(--accent); font-size: 12px; display: none; }
        .link-btn { background: none; color: var(--accent); font-size: 11px; cursor: pointer; text-decoration: underline; margin-top: 10px; }

        .app-container { display: flex; height: 100vh; }
        .sidebar { width: 240px; background: var(--primary); color: white; display: flex; flex-direction: column; flex-shrink: 0; }
        .sidebar-header { padding: 20px; border-bottom: 1px solid rgba(255,255,255,0.1); text-align: center; }
        .sidebar-menu { flex: 1; overflow-y: auto; padding: 0; list-style: none; margin: 0; }
        .sidebar-menu li { padding: 12px 20px; cursor: pointer; border-left: 4px solid transparent; display: flex; align-items: center; gap: 10px; font-size: 13px; }
        .sidebar-menu li:hover, .sidebar-menu li.active { background: rgba(255,255,255,0.1); border-left-color: var(--secondary); }
        
        .main-content { flex: 1; display: flex; flex-direction: column; overflow: hidden; }
        .top-bar { height: 60px; background: white; border-bottom: 1px solid #ddd; display: flex; align-items: center; justify-content: space-between; padding: 0 20px; box-shadow: 0 2px 5px rgba(0,0,0,0.05); z-index: 10; }
        .user-info { display: flex; align-items: center; gap: 10px; }
        
        .page-content { flex: 1; padding: 20px; overflow-y: auto; background: var(--bg); }
        .module-section { display: none; }
        .module-section.active { display: block; }

        /* Components */
        .card { background: white; border-radius: 8px; padding: 20px; box-shadow: 0 2px 5px rgba(0,0,0,0.05); margin-bottom: 20px; }
        .card-header { display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #eee; padding-bottom: 10px; margin-bottom: 15px; }
        .stats-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin-bottom: 20px; }
        .stat-box { background: white; padding: 15px; border-radius: 8px; border-left: 4px solid var(--secondary); box-shadow: 0 2px 5px rgba(0,0,0,0.05); display: flex; justify-content: space-between; align-items: center; }
        .stat-box h3 { margin: 0; color: var(--primary); font-size: 18px; } .stat-box p { margin: 5px 0 0; color: #777; font-size: 12px; }
        
        table { width: 100%; border-collapse: collapse; }
        th, td { padding: 10px; text-align: left; border-bottom: 1px solid #eee; font-size: 13px; vertical-align: top; }
        th { background: #f8f9fa; color: #555; position: sticky; top: 0; }
        .badge { padding: 3px 8px; border-radius: 10px; font-size: 10px; font-weight: bold; }
        .badge-success { background: #e8f5e9; color: #2e7d32; }
        .badge-warning { background: #fff8e1; color: #f9a825; }
        .badge-danger { background: #ffebee; color: #c62828; }
        .badge-info { background: #e3f2fd; color: #1565c0; }

        /* Tabs */
        .tabs { display: flex; gap: 5px; margin-bottom: 15px; border-bottom: 1px solid #ddd; padding-bottom: 5px; }
        .tab-btn { background: none; color: #666; border-radius: 0; padding: 8px 15px; border-bottom: 2px solid transparent; margin-bottom: -6px; }
        .tab-btn.active { border-bottom-color: var(--secondary); color: var(--secondary); font-weight: bold; background: none; }

        /* Modals */
        .modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.6); justify-content: center; align-items: flex-start; z-index: 1000; padding-top: 50px; overflow-y: auto; }
        .modal-content { background: white; padding: 25px; border-radius: 8px; width: 500px; max-width: 90%; position: relative; margin-bottom: 50px; }
        .close-btn { position: absolute; top: 10px; right: 15px; background: none; font-size: 20px; color: #888; cursor: pointer; }

        /* POS */
        .pos-grid { display: grid; grid-template-columns: 1fr 350px; gap: 20px; height: calc(100vh - 120px); }
        .product-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(140px, 1fr)); gap: 10px; align-content: start; overflow-y: auto; padding-right: 5px; max-height: 100%; }
        .product-card { background: white; padding: 10px; border-radius: 6px; text-align: center; cursor: pointer; border: 1px solid #eee; transition: 0.2s; position: relative; }
        .product-card:hover { border-color: var(--secondary); }
        .cart-panel { background: white; border-radius: 8px; display: flex; flex-direction: column; height: fit-content; max-height: 100%; }
        .cart-items { flex: 1; overflow-y: auto; padding: 15px; border-bottom: 1px solid #eee; max-height: 300px; }
        
        /* Pipeline */
        .pipeline { display: flex; gap: 10px; overflow-x: auto; padding-bottom: 10px; }
        .pipeline-stage { min-width: 250px; background: #f9f9f9; border-radius: 8px; padding: 10px; }
        .stage-header { font-weight: bold; margin-bottom: 10px; padding-bottom: 5px; border-bottom: 2px solid #ddd; font-size: 12px; text-transform: uppercase; color: #666; }
        .deal-card { background: white; padding: 10px; border-radius: 5px; margin-bottom: 8px; box-shadow: 0 1px 2px rgba(0,0,0,0.1); font-size: 12px; }
        
        /* Print */
        @media print { body * { visibility: hidden; } #printArea, #printArea * { visibility: visible; } #printArea { position: absolute; left: 0; top: 0; width: 100%; } }
    </style>
</head>
<body>

    <!-- Login Screen -->
    <div id="loginScreen" class="login-container">
        <div class="login-box">
            <h2 style="color: var(--primary);">BIGMAN <span style="color:var(--secondary)">HOLDINGS</span></h2>
            <p style="font-size:12px; color:#777;">Enterprise Resource Planning</p>
            <hr><br>
            <input type="password" id="pinInput" placeholder="Enter Admin PIN (1234)" maxlength="4">
            <button class="btn-login" onclick="attemptLogin()">ACCESS SYSTEM</button>
            <div id="loginError" class="error-msg">Invalid PIN. Try 1234.</div>
            <button class="link-btn" onclick="resetSystem()">Forgot Password / Reset System</button>
        </div>
    </div>

    <!-- Main App -->
    <div id="appScreen" class="app-container" style="display: none;">
        
        <!-- Sidebar -->
        <nav class="sidebar">
            <div class="sidebar-header">
                <h2 style="margin:0;">BIGMAN</h2>
                <small style="opacity:0.7;">Holdings Ltd</small>
            </div>
            <ul class="sidebar-menu">
                <li class="active" onclick="openModule('dashboard')"><i class="fas fa-tachometer-alt"></i> Dashboard</li>
                <li onclick="openModule('realestate')"><i class="fas fa-building"></i> Real Estate</li>
                <li onclick="openModule('electronics')"><i class="fas fa-cash-register"></i> POS (Electronics)</li>
                <li onclick="openModule('invoicing')"><i class="fas fa-file-invoice-dollar"></i> Invoicing</li>
                <li onclick="openModule('finance')"><i class="fas fa-chart-line"></i> Finance</li>
                <li onclick="openModule('hr')"><i class="fas fa-users-cog"></i> HR & Payroll</li>
                <li onclick="openModule('construction')"><i class="fas fa-hard-hat"></i> Construction</li>
            </ul>
            <div style="padding:15px; text-align:center; border-top:1px solid rgba(255,255,255,0.1);">
                <button onclick="logout()" style="background:none; color:white; border:1px solid rgba(255,255,255,0.3); width:100%; font-size:12px;"><i class="fas fa-sign-out-alt"></i> Logout</button>
            </div>
        </nav>

        <!-- Main Content -->
        <main class="main-content">
            <header class="top-bar">
                <h3 id="pageTitle" style="margin:0;">Dashboard</h3>
                <div class="user-info">
                    <span id="dateDisplay"></span>
                    <span style="font-weight:bold;">Sebastian Mwakalobo</span>
                    <span class="badge badge-success">Admin</span>
                </div>
            </header>

            <div class="page-content">

                <!-- Dashboard -->
                <section id="mod_dashboard" class="module-section active">
                    <div class="stats-grid">
                        <div class="stat-box" style="border-color: var(--success);"><div><h3 id="dash_revenue">TZS 0</h3><p>Total Revenue</p></div><i class="fas fa-coins stat-icon"></i></div>
                        <div class="stat-box" style="border-color: var(--accent);"><div><h3 id="dash_expenses">TZS 0</h3><p>Total Expenses</p></div><i class="fas fa-receipt stat-icon"></i></div>
                        <div class="stat-box" style="border-color: var(--primary);"><div><h3 id="dash_profit">TZS 0</h3><p>Net Profit</p></div><i class="fas fa-chart-pie stat-icon"></i></div>
                        <div class="stat-box" style="border-color: var(--warning);"><div><h3 id="dash_pending_tasks">0</h3><p>Pending Deals</p></div><i class="fas fa-tasks stat-icon"></i></div>
                    </div>
                    <div style="display: grid; grid-template-columns: 2fr 1fr; gap: 20px;">
                        <div class="card">
                            <div class="card-header"><h4>Recent Financial Activity</h4></div>
                            <div id="activityLog" style="max-height: 300px; overflow-y:auto;"></div>
                        </div>
                        <div class="card">
                            <div class="card-header"><h4>Alerts</h4></div>
                            <div id="alertsLog"></div>
                        </div>
                    </div>
                </section>

                <!-- Real Estate Module -->
                <section id="mod_realestate" class="module-section">
                    <div class="tabs">
                        <button class="tab-btn active" onclick="openReTab('properties')">Properties</button>
                        <button class="tab-btn" onclick="openReTab('tenants')">Tenants</button>
                        <button class="tab-btn" onclick="openReTab('pipeline')">Deals Pipeline</button>
                        <button class="tab-btn" onclick="openReTab('maintenance')">Maintenance</button>
                    </div>

                    <div id="re_properties" class="card">
                        <div class="card-header"><h4>Property Portfolio</h4><button class="btn-primary" onclick="openModal('propertyModal')"><i class="fas fa-plus"></i> Add Property</button></div>
                        <table><thead><tr><th>Name</th><th>Location</th><th>Type</th><th>Action</th></tr></thead><tbody id="propertyTable"></tbody></table>
                    </div>

                    <div id="re_tenants" class="card" style="display:none;">
                        <div class="card-header"><h4>Tenants</h4><button class="btn-primary" onclick="openModal('tenantModal')"><i class="fas fa-plus"></i> Add Tenant</button></div>
                        <table><thead><tr><th>Name</th><th>Property</th><th>Phone</th><th>Rent</th><th>Status</th><th>Action</th></tr></thead><tbody id="tenantTable"></tbody></table>
                    </div>

                    <div id="re_pipeline" class="card" style="display:none;">
                        <div class="card-header"><h4>Sales Pipeline</h4><button class="btn-primary" onclick="openModal('dealModal')"><i class="fas fa-plus"></i> New Lead</button></div>
                        <div class="pipeline" id="pipelineBoard"></div>
                    </div>

                    <div id="re_maintenance" class="card" style="display:none;">
                        <div class="card-header"><h4>Maintenance</h4><button class="btn-primary" onclick="openModal('maintModal')"><i class="fas fa-tools"></i> Log Issue</button></div>
                        <table><thead><tr><th>Property</th><th>Issue</th><th>Cost</th><th>Status</th><th>Action</th></tr></thead><tbody id="maintTable"></tbody></table>
                    </div>
                </section>

                <!-- POS Module -->
                <section id="mod_electronics" class="module-section">
                    <div class="card" style="margin-bottom:0; border:none; box-shadow:none;">
                        <div class="card-header" style="border:none; padding-bottom:0;">
                            <h4>Inventory & Sales</h4>
                            <button class="btn-outline" onclick="openModal('inventoryModal')"><i class="fas fa-boxes"></i> Manage Stock</button>
                        </div>
                    </div>
                    <div class="pos-grid">
                        <div><div class="product-grid" id="posProducts"></div></div>
                        <div class="cart-panel">
                            <div style="padding:15px; border-bottom:1px solid #eee; font-weight:bold; background:#f9f9f9;">Current Sale</div>
                            <div class="cart-items" id="cartItems"><p style="text-align:center; color:#ccc;">Empty</p></div>
                            <div style="padding:15px; background:#f9f9f9; font-size:18px; font-weight:bold; display:flex; justify-content:space-between;"><span>TOTAL:</span> <span id="cartTotal">TZS 0</span></div>
                            <div style="padding:15px;"><button class="btn-success" style="width:100%; padding:12px;" onclick="checkout()"><i class="fas fa-check"></i> Complete Sale</button></div>
                        </div>
                    </div>
                </section>

                <!-- Invoicing -->
                <section id="mod_invoicing" class="module-section">
                    <div class="card">
                        <div class="card-header"><h4>Service Invoice</h4></div>
                        <div class="form-row">
                            <div class="form-group" style="flex:2"><label>Client</label><input type="text" id="inv_client"></div>
                            <div class="form-group"><label>Division</label><select id="inv_division"><option>Construction</option><option>IT</option></select></div>
                        </div>
                        <div id="invoiceItems"><div class="form-row" style="align-items:end;"><div class="form-group" style="flex:3"><label>Description</label><input type="text" class="inv_desc"></div><div class="form-group" style="flex:1"><label>Amount</label><input type="number" class="inv_amt" onchange="calcInvoice()"></div></div></div>
                        <button onclick="addInvoiceRow()" class="btn-outline" style="font-size:12px; margin-bottom:15px;">+ Add Row</button>
                        <div style="text-align:right; border-top:1px solid #eee; padding-top:15px;"><h3>TOTAL: <span id="invTotal">TZS 0</span></h3><button class="btn-success" onclick="generateInvoice()"><i class="fas fa-print"></i> Generate</button></div>
                    </div>
                </section>

                <!-- Finance -->
                <section id="mod_finance" class="module-section">
                    <div class="card">
                        <div class="card-header"><h4>Record Expense</h4></div>
                        <div class="form-row">
                            <div class="form-group"><label>Category</label><select id="exp_cat"><option>Transport</option><option>Salaries</option><option>Materials</option></select></div>
                            <div class="form-group"><label>Amount</label><input type="number" id="exp_amt"></div>
                            <div class="form-group"><label>Description</label><input type="text" id="exp_desc"></div>
                            <div class="form-group" style="flex:0.5"><label>&nbsp;</label><button class="btn-danger" onclick="addExpense()">Deduct</button></div>
                        </div>
                        <table style="margin-top:20px;"><thead><tr><th>Date</th><th>Category</th><th>Description</th><th>Amount</th></tr></thead><tbody id="expenseTable"></tbody></table>
                    </div>
                </section>

                <!-- HR -->
                <section id="mod_hr" class="module-section">
                    <div class="card">
                        <div class="card-header"><h4>Staff & Payroll</h4><button class="btn-primary" onclick="openModal('staffModal')"><i class="fas fa-plus"></i> Add Staff</button></div>
                        <table><thead><tr><th>Name</th><th>Role</th><th>Division</th><th>Salary</th><th>Action</th></tr></thead><tbody id="staffTable"></tbody></table>
                    </div>
                </section>

                <!-- Construction -->
                <section id="mod_construction" class="module-section">
                     <div class="card">
                        <div class="card-header"><h4>Projects</h4><button class="btn-primary" onclick="openModal('projectModal')"><i class="fas fa-hard-hat"></i> New Project</button></div>
                        <table><thead><tr><th>Name</th><th>Client</th><th>Budget</th><th>Status</th></tr></thead><tbody id="projectTable"></tbody></table>
                    </div>
                </section>

            </div>
        </main>
    </div>

    <!-- MODALS -->
    <div id="propertyModal" class="modal"><div class="modal-content"><span class="close-btn" onclick="closeModal('propertyModal')">&times;</span><div class="modal-header"><h3>Add Property</h3></div><label>Name</label><input type="text" id="prop_name"><label>Location</label><input type="text" id="prop_loc"><button class="btn-success" style="width:100%;" onclick="saveProperty()">Save</button></div></div>
    <div id="tenantModal" class="modal"><div class="modal-content"><span class="close-btn" onclick="closeModal('tenantModal')">&times;</span><div class="modal-header"><h3>Add Tenant</h3></div><label>Name</label><input type="text" id="tenant_name"><label>Phone</label><input type="text" id="tenant_phone"><label>Property</label><select id="tenant_prop" style="width:100%; padding:8px; margin-bottom:15px;"></select><label>Rent</label><input type="number" id="tenant_rent"><button class="btn-success" style="width:100%;" onclick="saveTenant()">Save</button></div></div>
    <div id="staffModal" class="modal"><div class="modal-content"><span class="close-btn" onclick="closeModal('staffModal')">&times;</span><div class="modal-header"><h3>Add Staff</h3></div><label>Name</label><input type="text" id="staff_name"><label>Role</label><input type="text" id="staff_role"><label>Division</label><select id="staff_div"><option>Construction</option><option>Real Estate</option></select><label>Salary</label><input type="number" id="staff_salary"><button class="btn-success" style="width:100%;" onclick="saveStaff()">Save</button></div></div>
    <div id="inventoryModal" class="modal"><div class="modal-content"><span class="close-btn" onclick="closeModal('inventoryModal')">&times;</span><div class="modal-header"><h3>Inventory</h3></div><label>Item Name</label><input type="text" id="inv_name"><label>Price</label><input type="number" id="inv_price"><label>Stock</label><input type="number" id="inv_stock"><button class="btn-success" style="width:100%;" onclick="saveProduct()">Add</button><hr><table><thead><tr><th>Name</th><th>Stock</th></tr></thead><tbody id="inventoryList"></tbody></table></div></div>
    <div id="dealModal" class="modal"><div class="modal-content"><span class="close-btn" onclick="closeModal('dealModal')">&times;</span><div class="modal-header"><h3>New Lead</h3></div><label>Client</label><input type="text" id="deal_client"><label>Property Interest</label><input type="text" id="deal_prop"><label>Price</label><input type="number" id="deal_price"><label>Stage</label><select id="deal_stage"><option>Inquiry</option><option>Viewing</option><option>Negotiation</option></select><button class="btn-success" style="width:100%;" onclick="saveDeal()">Add</button></div></div>
    <div id="maintModal" class="modal"><div class="modal-content"><span class="close-btn" onclick="closeModal('maintModal')">&times;</span><div class="modal-header"><h3>Maintenance</h3></div><label>Property</label><select id="maint_prop" style="width:100%; margin-bottom:15px;"></select><label>Issue</label><input type="text" id="maint_desc"><label>Cost</label><input type="number" id="maint_cost"><button class="btn-success" style="width:100%;" onclick="saveMaint()">Log Issue</button></div></div>
    <div id="projectModal" class="modal"><div class="modal-content"><span class="close-btn" onclick="closeModal('projectModal')">&times;</span><div class="modal-header"><h3>New Project</h3></div><label>Name</label><input type="text" id="proj_name"><label>Client</label><input type="text" id="proj_client"><label>Budget</label><input type="number" id="proj_budget"><button class="btn-success" style="width:100%;" onclick="saveProject()">Create</button></div></div>
    <div id="printModal" class="modal"><div class="modal-content" style="width:400px;" id="printArea"><div id="printContent"></div><div style="margin-top:15px; display:flex; gap:10px;"><button class="btn-primary" style="flex:1" onclick="window.print()">Print</button><button class="btn-success" style="flex:1; background:#25D366;" onclick="sendWhatsApp()">WhatsApp</button></div><button onclick="closeModal('printModal')" style="width:100%; margin-top:5px; background:#eee; color:#333;">Close</button></div></div>

    <script>
        // --- Configuration ---
        const ADMIN_PIN = "1234";
        const COMPANY = { name: "Bigman Holdings Ltd", tin: "152523637", phone: "0683601521", bank: "CRDB Bank", acc: "10242650692", accName: "Sebastian Mwakalobo" };
        
        // --- Database Structure ---
        let DB = {
            properties: [], tenants: [], staff: [], products: [],
            sales: [], invoices: [], activities: [], expenses: [],
            deals: [], maintenance: [], projects: []
        };

        function loadDB() {
            try {
                const stored = localStorage.getItem('bigman_db_v4');
                if (stored) {
                    let parsed = JSON.parse(stored);
                    // Merge to ensure new fields exist
                    DB = { ...DB, ...parsed };
                } else {
                    // First Run - Add Sample Data
                    DB.products = [ { id: 1, name: "Samsung A55", price: 850000, stock: 10 }, { id: 2, name: "HP Laptop", price: 1250000, stock: 5 } ];
                    DB.deals = [ { id: 1, client: "Mr. Hamisi", prop: "Mbezi Villa", price: 180000000, stage: "Negotiation", date: new Date().toLocaleDateString() } ];
                    saveDB();
                }
            } catch (e) {
                console.error("Error loading database", e);
                alert("System loaded with fresh data.");
            }
        }

        function saveDB() {
            try {
                localStorage.setItem('bigman_db_v4', JSON.stringify(DB));
                updateDashboard();
            } catch(e) {
                console.error("Error saving database", e);
            }
        }

        function resetSystem() {
            if(confirm("This will delete all data. Are you sure?")) {
                localStorage.removeItem('bigman_db_v4');
                location.reload();
            }
        }

        // --- Logic ---
        function attemptLogin() {
            const inputVal = document.getElementById('pinInput').value;
            if (inputVal === ADMIN_PIN) {
                document.getElementById('loginScreen').style.display = 'none';
                document.getElementById('appScreen').style.display = 'flex';
                initApp();
            } else {
                document.getElementById('loginError').style.display = 'block';
            }
        }
        function logout() { location.reload(); }

        function initApp() {
            loadDB();
            renderAll();
            document.getElementById('dateDisplay').innerText = new Date().toLocaleDateString('en-US', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' });
        }

        function renderAll() {
            renderProperties(); renderTenants(); renderStaff(); renderPOS(); renderExpenses(); renderDeals(); renderMaintenance(); renderProjects();
            updateDashboard();
        }

        function openModule(name) {
            document.querySelectorAll('.module-section').forEach(el => el.classList.remove('active'));
            document.getElementById('mod_' + name).classList.add('active');
            document.querySelectorAll('.sidebar-menu li').forEach(el => el.classList.remove('active'));
            event.target.closest('li').classList.add('active');
            document.getElementById('pageTitle').innerText = name.charAt(0).toUpperCase() + name.slice(1);
        }

        function openReTab(tabName) {
            document.querySelectorAll('[id^="re_"]').forEach(el => el.style.display = 'none');
            document.getElementById('re_' + tabName).style.display = 'block';
            document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');
        }

        // --- Dashboard ---
        function updateDashboard() {
            let revenue = DB.sales.reduce((a, b) => a + b.total, 0);
            let expenses = DB.expenses.reduce((a, b) => a + b.amount, 0);
            let profit = revenue - expenses;
            
            document.getElementById('dash_revenue').innerText = "TZS " + revenue.toLocaleString();
            document.getElementById('dash_expenses').innerText = "TZS " + expenses.toLocaleString();
            document.getElementById('dash_profit').innerText = "TZS " + profit.toLocaleString();
            
            let logHtml = '';
            DB.activities.slice(-10).reverse().forEach(a => {
                logHtml += `<p style="border-bottom:1px solid #f0f0f0; padding:5px 0;"><i class="fas fa-circle" style="font-size:6px; color:var(--secondary); margin-right:5px;"></i> <b>${a.type || 'Activity'}:</b> ${a.text} <small style="float:right; color:#999;">${a.time.split(',')[0]}</small></p>`;
            });
            document.getElementById('activityLog').innerHTML = logHtml || '<p style="color:#ccc">No activity.</p>';

            let pendingDeals = DB.deals.filter(d => d.stage !== 'Closed');
            document.getElementById('dash_pending_tasks').innerText = pendingDeals.length;
            document.getElementById('alertsLog').innerHTML = pendingDeals.length > 0 ? `<p style="color:var(--warning)"><i class="fas fa-handshake"></i> ${pendingDeals.length} active deals in pipeline.</p>` : '<p style="color:#27ae60">All systems normal.</p>';
        }

        // --- Real Estate ---
        function renderProperties() {
            let html = '';
            DB.properties.forEach(p => { html += `<tr><td>${p.name}</td><td>${p.location}</td><td>${p.type}</td><td><button class="btn-danger" style="font-size:10px;" onclick="deleteData('properties', ${p.id})">Del</button></td></tr>`; });
            document.getElementById('propertyTable').innerHTML = html || '<tr><td colspan="4">No properties.</td></tr>';
            let opts = '<option value="">Select</option>'; DB.properties.forEach(p => opts += `<option value="${p.name}">${p.name}</option>`);
            document.getElementById('tenant_prop').innerHTML = opts;
            document.getElementById('maint_prop').innerHTML = opts;
        }
        function saveProperty() { let p = { id: Date.now(), name: val('prop_name'), location: val('prop_loc'), type: 'Property' }; DB.properties.push(p); logAct('Property', `Added ${p.name}`); saveDB(); renderProperties(); closeModal('propertyModal'); }

        function renderTenants() {
            let html = '';
            DB.tenants.forEach(t => { html += `<tr><td>${t.name}</td><td>${t.prop}</td><td>${t.phone}</td><td>${t.rent.toLocaleString()}</td><td><span class="badge ${t.paid ? 'badge-success' : 'badge-warning'}">${t.paid ? 'Paid' : 'Due'}</span></td><td><button class="btn-success" style="font-size:10px;" onclick="payRent(${t.id})">Pay</button> <button class="btn-danger" style="font-size:10px;" onclick="deleteData('tenants', ${t.id})">Del</button></td></tr>`; });
            document.getElementById('tenantTable').innerHTML = html || '<tr><td colspan="6">No tenants.</td></tr>';
        }
        function saveTenant() { let t = { id: Date.now(), name: val('tenant_name'), phone: val('tenant_phone'), prop: val('tenant_prop'), rent: parseFloat(val('tenant_rent')), paid: false }; DB.tenants.push(t); logAct('Tenant', `Added ${t.name}`); saveDB(); renderTenants(); closeModal('tenantModal'); }
        function payRent(id) { let t = DB.tenants.find(x => x.id === id); if(t) { t.paid = true; let receipt = { client: t.name, total: t.rent, type: 'Rent', date: new Date().toLocaleString(), ref: 'RNT-' + Date.now(), details: `Rent for ${t.prop}` }; DB.sales.push(receipt); logAct('Income', `Rent ${t.rent.toLocaleString()} from ${t.name}`); saveDB(); renderTenants(); printReceipt(receipt); } }

        // --- Deals ---
        function renderDeals() {
            const stages = ['Inquiry', 'Viewing', 'Negotiation', 'Closed'];
            let html = '';
            stages.forEach(stage => {
                let deals = DB.deals.filter(d => d.stage === stage);
                html += `<div class="pipeline-stage"><div class="stage-header">${stage} (${deals.length})</div>`;
                deals.map(d => { html += `<div class="deal-card"><h5>${d.client}</h5><p>${d.prop}</p><p><b>TZS ${d.price.toLocaleString()}</b></p><div style="margin-top:5px;">${stage !== 'Closed' ? `<button class="btn-outline" style="font-size:10px;" onclick="nextStage(${d.id})">Next Stage &rarr;</button>` : '<span class="badge badge-success">Done</span>'}</div></div>`; }).join('');
                html += `</div>`;
            });
            document.getElementById('pipelineBoard').innerHTML = html;
        }
        function saveDeal() { let d = { id: Date.now(), client: val('deal_client'), prop: val('deal_prop'), price: parseFloat(val('deal_price')), stage: val('deal_stage'), date: new Date().toLocaleDateString() }; DB.deals.push(d); logAct('Lead', `New Lead: ${d.client}`); saveDB(); renderDeals(); closeModal('dealModal'); }
        function nextStage(id) { let d = DB.deals.find(x => x.id === id); const stages = ['Inquiry', 'Viewing', 'Negotiation', 'Closed']; let idx = stages.indexOf(d.stage); if(idx < stages.length - 1) { d.stage = stages[idx + 1]; if(d.stage === 'Closed') { DB.sales.push({ client: d.client, total: d.price, type: 'Property Sale', date: new Date().toLocaleString(), ref: 'SAL-' + Date.now(), details: d.prop }); logAct('Sale', `Deal Closed: ${d.prop}`); } saveDB(); renderDeals(); } }

        // --- Maintenance ---
        function renderMaintenance() { let html = ''; DB.maintenance.forEach(m => { html += `<tr><td>${m.prop}</td><td>${m.desc}</td><td>${m.cost.toLocaleString()}</td><td><span class="badge badge-${m.status === 'Done' ? 'success' : 'info'}">${m.status}</span></td><td>${m.status !== 'Done' ? `<button class="btn-success" style="font-size:10px;" onclick="completeMaint(${m.id})">Fix Done</button>` : ''}</td></tr>`; }); document.getElementById('maintTable').innerHTML = html || '<tr><td colspan="5">No issues.</td></tr>'; }
        function saveMaint() { let m = { id: Date.now(), prop: val('maint_prop'), desc: val('maint_desc'), cost: parseFloat(val('maint_cost')), status: 'Pending' }; DB.maintenance.push(m); logAct('Repair', `Issue at ${m.prop}`); saveDB(); renderMaintenance(); closeModal('maintModal'); }
        function completeMaint(id) { let m = DB.maintenance.find(x => x.id === id); if(m) { m.status = 'Done'; DB.expenses.push({ id: Date.now(), cat: 'Maintenance', desc: `Repair: ${m.desc}`, amount: m.cost, date: new Date().toLocaleString() }); logAct('Repair', `Fixed ${m.prop}`); saveDB(); renderMaintenance(); renderExpenses(); } }

        // --- POS ---
        let cart = [];
        function renderPOS() { let html = ''; DB.products.forEach(p => { html += `<div class="product-card" onclick="addToCart(${p.id})"><span class="stock-badge">${p.stock} left</span><i class="fas fa-box" style="font-size:24px; color:#ddd; margin:10px 0;"></i><h4 style="margin:0;">${p.name}</h4><p style="color:var(--secondary); font-weight:bold;">${p.price.toLocaleString()}</p></div>`; }); document.getElementById('posProducts').innerHTML = html; }
        function renderInventoryList() { let html = ''; DB.products.forEach(p => { html += `<tr><td>${p.name}</td><td>${p.stock}</td></tr>`; }); document.getElementById('inventoryList').innerHTML = html; }
        function saveProduct() { let p = { id: Date.now(), name: val('inv_name'), price: parseFloat(val('inv_price')), stock: parseInt(val('inv_stock')) }; DB.products.push(p); saveDB(); renderPOS(); renderInventoryList(); }
        function addToCart(id) { let product = DB.products.find(p => p.id === id); let existing = cart.find(c => c.id === id); if (existing) existing.qty++; else cart.push({ ...product, qty: 1 }); renderCart(); }
        function renderCart() { let html = ''; let total = 0; cart.forEach(c => { total += c.price * c.qty; html += `<div class="cart-item"><div><strong>${c.name}</strong><br><small>${c.qty} x ${c.price.toLocaleString()}</small></div><div><strong>${(c.price * c.qty).toLocaleString()}</strong></div></div>`; }); document.getElementById('cartItems').innerHTML = html || '<p style="text-align:center; color:#ccc;">Empty</p>'; document.getElementById('cartTotal').innerText = "TZS " + total.toLocaleString(); }
        function checkout() { if (cart.length === 0) return alert("Empty"); let total = cart.reduce((a, b) => a + (b.price * b.qty), 0); cart.forEach(c => { let p = DB.products.find(x => x.id === c.id); if(p) p.stock -= c.qty; }); let sale = { client: "Walk-in", total: total, type: "Electronics", date: new Date().toLocaleString(), ref: "POS-" + Date.now(), details: cart.map(c => `${c.name} (${c.qty})`).join(", ") }; DB.sales.push(sale); logAct('Sale', `Sold items for ${total.toLocaleString()}`); saveDB(); printReceipt(sale); cart = []; renderCart(); renderPOS(); }

        // --- Finance ---
        function renderExpenses() { let html = ''; DB.expenses.slice(-10).reverse().forEach(e => { html += `<tr><td>${e.date.split(',')[0]}</td><td>${e.cat}</td><td>${e.desc}</td><td style="color:var(--accent)">-${e.amount.toLocaleString()}</td></tr>`; }); document.getElementById('expenseTable').innerHTML = html; }
        function addExpense() { let e = { id: Date.now(), cat: val('exp_cat'), amount: parseFloat(val('exp_amt')), desc: val('exp_desc'), date: new Date().toLocaleString() }; DB.expenses.push(e); logAct('Expense', `Spent ${e.amount.toLocaleString()}`); saveDB(); renderExpenses(); }

        // --- Invoicing ---
        function addInvoiceRow() { let div = document.createElement('div'); div.className = "form-row"; div.style.marginTop = "5px"; div.innerHTML = `<div class="form-group" style="flex:3"><input type="text" class="inv_desc" placeholder="Item"></div><div class="form-group" style="flex:1"><input type="number" class="inv_amt" placeholder="Amount" onchange="calcInvoice()"></div>`; document.getElementById('invoiceItems').appendChild(div); }
        function calcInvoice() { let t = 0; document.querySelectorAll('.inv_amt').forEach(i => t += parseFloat(i.value || 0)); document.getElementById('invTotal').innerText = "TZS " + t.toLocaleString(); }
        function generateInvoice() { let items = []; let total = 0; document.querySelectorAll('.form-row').forEach(row => { let desc = row.querySelector('.inv_desc')?.value; let amt = row.querySelector('.inv_amt')?.value; if (desc && amt) { items.push({ desc, amt }); total += parseFloat(amt); } }); if(items.length === 0) return alert("Add items"); let inv = { id: Date.now(), client: val('inv_client'), total: total, type: val('inv_division'), date: new Date().toLocaleString(), ref: "INV-" + Date.now(), details: items }; DB.invoices.push(inv); DB.sales.push(inv); logAct('Invoice', `Created for ${val('inv_client')}`); saveDB(); printReceipt(inv, true); }

        // --- HR & Projects ---
        function renderStaff() { let html = ''; DB.staff.forEach(s => { html += `<tr><td>${s.name}</td><td>${s.role}</td><td>${s.div}</td><td>${s.salary.toLocaleString()}</td><td><button class="btn-danger" style="font-size:10px;" onclick="deleteData('staff', ${s.id})">Del</button></td></tr>`; }); document.getElementById('staffTable').innerHTML = html || '<tr><td colspan="5">No staff.</td></tr>'; }
        function saveStaff() { let s = { id: Date.now(), name: val('staff_name'), role: val('staff_role'), div: val('staff_div'), salary: parseFloat(val('staff_salary')) }; DB.staff.push(s); saveDB(); renderStaff(); closeModal('staffModal'); }
        function renderProjects() { let html = ''; DB.projects.forEach(p => { html += `<tr><td>${p.name}</td><td>${p.client}</td><td>${p.budget.toLocaleString()}</td><td><span class="badge badge-info">Ongoing</span></td></tr>`; }); document.getElementById('projectTable').innerHTML = html || '<tr><td colspan="4">No projects.</td></tr>'; }
        function saveProject() { let p = { id: Date.now(), name: val('proj_name'), client: val('proj_client'), budget: parseFloat(val('proj_budget')) }; DB.projects.push(p); saveDB(); renderProjects(); closeModal('projectModal'); }

        // --- Utils ---
        function val(id) { return document.getElementById(id).value; }
        function logAct(type, text) { DB.activities.push({ type: type, text: text, time: new Date().toLocaleString() }); }
        function printReceipt(data, isInvoice = false) {
            let content = `<div style="text-align:center; border-bottom:1px dashed #ccc; padding-bottom:10px;"><h2 style="margin:0; font-size:20px;">${COMPANY.name}</h2><small>${COMPANY.tin} | ${COMPANY.phone}</small><br><small>Bank: ${COMPANY.bank} - ${COMPANY.acc}</small></div><div style="margin:15px 0;"><p><b>Client:</b> ${data.client}</p><p><b>Date:</b> ${data.date}</p><p><b>Ref:</b> ${data.ref}</p></div><div style="border-top:1px dashed #ccc; border-bottom:1px dashed #ccc; padding:10px 0;">`;
            if(isInvoice && Array.isArray(data.details)) { data.details.forEach(i => { content += `<div style="display:flex; justify-content:space-between;"><span>${i.desc}</span><span>TZS ${parseFloat(i.amt).toLocaleString()}</span></div>`; }); } else { content += `<p>${data.details}</p>`; }
            content += `</div><div style="margin-top:10px; text-align:right;"><h3>TOTAL: TZS ${data.total.toLocaleString()}</h3></div>`;
            document.getElementById('printContent').innerHTML = content;
            openModal('printModal');
        }
        function sendWhatsApp() { let text = encodeURIComponent(`*${COMPANY.name}*\nRef: ${document.getElementById('printContent').querySelector('p:nth-child(3)').innerText.split(': ')[1]}\nTotal: ${document.getElementById('printContent').querySelector('h3').innerText}\n\nBank: ${COMPANY.bank}\nAcc: ${COMPANY.acc}\nName: ${COMPANY.accName}`); window.open(`https://wa.me/?text=${text}`, '_blank'); }
        function openModal(id) { document.getElementById(id).style.display = 'flex'; if(id === 'inventoryModal') renderInventoryList(); }
        function closeModal(id) { document.getElementById(id).style.display = 'none'; }
        function deleteData(type, id) { if(confirm("Delete?")) { DB[type] = DB[type].filter(x => x.id !== id); saveDB(); renderAll(); } }

        // Init
        loadDB();
    </script>
</body>
</html>
