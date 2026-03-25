# bigmanholdings-
Bigman Holdings 
-- Client Table (Shared across all 5 divisions)
CREATE TABLE Clients (
    client_id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL,
    phone TEXT UNIQUE,
    category TEXT, -- Buyer, Tenant, Investor, etc.
    total_spent REAL DEFAULT 0
);

-- Deals & Sales Table
CREATE TABLE Transactions (
    trans_id INTEGER PRIMARY KEY AUTOINCREMENT,
    division TEXT, -- 'Real Estate', 'Construction', etc.
    client_id INTEGER,
    total_amount REAL,
    status TEXT, -- 'Pending', 'Closed', 'Negotiation'
    pin_used TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);import React, { useState } from 'react';

const LoginScreen = ({ onLogin }) => {
  const [pin, setPin] = useState('');
  const [error, setError] = useState('');

  const handleLogin = (e) => {
    e.preventDefault();
    if (pin === '1234') {
      onLogin();
    } else {
      setError('❌ Incorrect Admin PIN. Access Denied.');
      setPin('');
    }
  };

  return (
    <div style={styles.loginContainer}>
      <div style={styles.loginBox}>
        <h1 style={styles.brand}>BIGMAN HOLDINGS LTD</h1>
        <p style={styles.subtitle}>Unified Management System</p>
        <form onSubmit={handleLogin}>
          <input 
            type="password" 
            placeholder="Enter Admin PIN" 
            value={pin}
            onChange={(e) => setPin(e.target.value)}
            style={styles.input}
            maxLength={4}
          />
          <button type="submit" style={styles.button}>Unlock System</button>
        </form>
        {error && <p style={styles.error}>{error}</p>}
      </div>
    </div>
  );
};const Dashboard = () => {
  const divisions = [
    { name: 'Real Estate', icon: '🏠', stats: '12 Units Overdue' },
    { name: 'Construction', icon: '🏗️', stats: '3 Active Sites' },
    { name: 'Car Dealership', icon: '🚗', stats: '8 Vehicles in Stock' },
    { name: 'Electronics', icon: '📱', stats: 'Low Stock: 4 items' },
    { name: 'IT Solutions', icon: '💻', stats: '2 Pending Invoices' },
  ];

  return (
    <div style={styles.dashboard}>
      <header style={styles.header}>
        <h2>Welcome, Admin</h2>
        <div style={styles.statusIndicator}>
          {navigator.onLine ? '🌐 Online (WhatsApp Active)' : '📡 Offline (Messages Queued)'}
        </div>
      </header>

      <div style={styles.statsGrid}>
        <div style={styles.totalCard}>
          <h3>Total Revenue (Monthly)</h3>
          <h1>TZS 45,250,000</h1>
        </div>
      </div>

      <div style={styles.grid}>
        {divisions.map((div) => (
          <div key={div.name} style={styles.card}>
            <span style={{ fontSize: '30px' }}>{div.icon}</span>
            <h4>{div.name}</h4>
            <p>{div.stats}</p>
            <button style={styles.openBtn}>Open Division</button>
          </div>
        ))}
      </div>
    </div>
  );
};const styles = {
  loginContainer: { height: '100vh', display: 'flex', justifyContent: 'center', alignItems: 'center', backgroundColor: '#f0f2f5' },
  loginBox: { padding: '40px', backgroundColor: '#fff', borderRadius: '12px', boxShadow: '0 4px 20px rgba(0,0,0,0.1)', textAlign: 'center', width: '350px' },
  brand: { margin: 0, color: '#1a3a5f', fontSize: '24px' },
  input: { width: '100%', padding: '12px', margin: '20px 0', borderRadius: '8px', border: '1px solid #ddd', textAlign: 'center', fontSize: '18px', letterSpacing: '8px' },
  button: { width: '100%', padding: '12px', backgroundColor: '#1a3a5f', color: '#fff', border: 'none', borderRadius: '8px', cursor: 'pointer', fontWeight: 'bold' },
  error: { color: 'red', marginTop: '10px', fontSize: '14px' },
  dashboard: { padding: '20px', fontFamily: 'Arial, sans-serif' },
  header: { display: 'flex', justifyContent: 'space-between', alignItems: 'center', marginBottom: '30px' },
  grid: { display: 'grid', gridTemplateColumns: 'repeat(auto-fit, minmax(200px, 1fr))', gap: '20px' },
  card: { padding: '20px', backgroundColor: '#fff', borderRadius: '10px', border: '1px solid #eee', textAlign: 'center', boxShadow: '0 2px 8px rgba(0,0,0,0.05)' },
  openBtn: { marginTop: '10px', padding: '8px 16px', backgroundColor: '#f0f2f5', border: 'none', borderRadius: '5px', cursor: 'pointer' },
  totalCard: { backgroundColor: '#1a3a5f', color: '#fff', padding: '30px', borderRadius: '15px', marginBottom: '20px' }
};import React, { useState } from 'react';

const RealEstateModule = () => {
  const [properties, setProperties] = useState([
    { id: 1, name: 'Unit 4B - Sinza Block', type: 'Rental', price: 450000, status: 'Occupied', tenant: 'Joseph Kimaro', dueDate: '1st April' },
    { id: 2, name: '3-Bedroom House - Mbezi', type: 'Sale', price: 180000000, status: 'Negotiation', tenant: 'Mr. Hamisi Juma', dueDate: 'N/A' },
    { id: 3, name: 'Unit 2A - Posta', type: 'Rental', price: 600000, status: 'Vacant', tenant: 'None', dueDate: 'N/A' }
  ]);

  return (
    <div style={styles.container}>
      <header style={styles.header}>
        <h3>Real Estate Division</h3>
        <button style={styles.addBtn}>+ Add New Property</button>
      </header>

      <div style={styles.tableContainer}>
        <table style={styles.table}>
          <thead>
            <tr style={styles.tableHeader}>
              <th>Property Name</th>
              <th>Type</th>
              <th>Price (TZS)</th>
              <th>Status</th>
              <th>Tenant/Buyer</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            {properties.map((p) => (
              <tr key={p.id} style={styles.row}>
                <td>{p.name}</td>
                <td><span style={styles.badge}>{p.type}</span></td>
                <td>{p.price.toLocaleString()}</td>
                <td style={{ color: p.status === 'Vacant' ? 'red' : 'green' }}>{p.status}</td>
                <td>{p.tenant}</td>
                <td>
                  <button style={styles.invoiceBtn} onClick={() => alert('Generating Invoice for ' + p.tenant)}>
                    📄 Invoice
                  </button>
                </td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
    </div>
  );
};const styles = {
  container: { padding: '20px', backgroundColor: '#fff', borderRadius: '12px' },
  header: { display: 'flex', justifyContent: 'space-between', alignItems: 'center', marginBottom: '20px' },
  addBtn: { backgroundColor: '#27ae60', color: '#fff', border: 'none', padding: '10px 20px', borderRadius: '6px', cursor: 'pointer' },
  tableContainer: { overflowX: 'auto' },
  table: { width: '100%', borderCollapse: 'collapse', textAlign: 'left' },
  tableHeader: { backgroundColor: '#f8f9fa', borderBottom: '2px solid #eee' },
  row: { borderBottom: '1px solid #eee', height: '50px' },
  badge: { backgroundColor: '#e1f5fe', color: '#01579b', padding: '4px 8px', borderRadius: '4px', fontSize: '12px' },
  invoiceBtn: { backgroundColor: '#1a3a5f', color: '#fff', border: 'none', padding: '6px 12px', borderRadius: '4px', cursor: 'pointer' }
};import React, { useState } from 'react';

const LeadManagement = () => {
  const [leads, setLeads] = useState([
    { id: 1, name: 'Amina Hassan', interest: '4-Bedroom House', budget: '200M TZS', status: 'Qualified', lastContact: '14 March' },
    { id: 2, name: 'Said Bakari', interest: 'Office Space Sinza', budget: '1.2M Rent', status: 'New', lastContact: 'Today' },
    { id: 3, name: 'Fatma Juma', interest: 'Plot in Kigamboni', budget: '45M TZS', status: 'Viewing Scheduled', lastContact: 'Yesterday' }
  ]);

  const getStatusColor = (status) => {
    switch(status) {
      case 'New': return '#3498db'; // Blue
      case 'Qualified': return '#f39c12'; // Orange
      case 'Viewing Scheduled': return '#e67e22'; // Dark Orange
      case 'Closed': return '#27ae60'; // Green
      default: return '#95a5a6';
    }
  };

  return (
    <div style={styles.leadContainer}>
      <div style={styles.header}>
        <h3>Lead Pipeline (Bigman Holdings)</h3>
        <p>Total Active Leads: {leads.length}</p>
      </div>

      <div style={styles.pipelineGrid}>
        {leads.map((lead) => (
          <div key={lead.id} style={styles.leadCard}>
            <div style={{...styles.statusBar, backgroundColor: getStatusColor(lead.status)}}></div>
            <h4>{lead.name}</h4>
            <p><strong>Interested in:</strong> {lead.interest}</p>
            <p><strong>Budget:</strong> {lead.budget}</p>
            <p style={styles.contactDate}>Last Contact: {lead.lastContact}</p>
            
            <div style={styles.actionRow}>
              <button style={styles.callBtn}>📞 Call</button>
              <button style={styles.waBtn}>📱 WhatsApp</button>
              <button style={styles.editBtn}>Update Status</button>
            </div>
          </div>
        ))}
      </div>
    </div>
  );
};const styles = {
  leadContainer: { padding: '20px', backgroundColor: '#f4f7f6', minHeight: '100vh' },
  header: { marginBottom: '20px', borderBottom: '2px solid #1a3a5f', paddingBottom: '10px' },
  pipelineGrid: { display: 'grid', gridTemplateColumns: 'repeat(auto-fit, minmax(300px, 1fr))', gap: '15px' },
  leadCard: { backgroundColor: '#fff', padding: '15px', borderRadius: '8px', boxShadow: '0 2px 5px rgba(0,0,0,0.1)', position: 'relative' },
  statusBar: { height: '5px', width: '100%', position: 'absolute', top: 0, left: 0, borderRadius: '8px 8px 0 0' },
  contactDate: { fontSize: '12px', color: '#7f8c8d', fontStyle: 'italic' },
  actionRow: { marginTop: '15px', display: 'flex', gap: '5px', flexWrap: 'wrap' },
  callBtn: { backgroundColor: '#f1f2f6', border: '1px solid #ddd', padding: '5px 10px', borderRadius: '4px', cursor: 'pointer' },
  waBtn: { backgroundColor: '#25D366', color: '#fff', border: 'none', padding: '5px 10px', borderRadius: '4px', cursor: 'pointer' },
  editBtn: { backgroundColor: '#1a3a5f', color: '#fff', border: 'none', padding: '5px 10px', borderRadius: '4px', cursor: 'pointer' }
};import React, { useState, useEffect } from 'react';

const RentTracker = () => {
  const [tenants, setTenants] = useState([
    { id: 1, name: 'Joseph Kimaro', unit: '4B Sinza', rent: 450000, dueDate: '2026-03-01', status: 'Paid' },
    { id: 2, name: 'Anna Mengi', unit: '1A Posta', rent: 600000, dueDate: '2026-03-20', status: 'Unpaid' },
    { id: 3, name: 'David Luvanda', unit: '3C Mbezi', rent: 850000, dueDate: '2026-03-24', status: 'Unpaid' }
  ]);

  const calculateLateInfo = (dueDate, status) => {
    if (status === 'Paid') return { days: 0, penalty: 0 };
    
    const today = new Date();
    const due = new Date(dueDate);
    const diffTime = today - due;
    const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
    
    return diffDays > 0 ? { days: diffDays, penalty: diffDays * 5000 } : { days: 0, penalty: 0 };
  };

  return (
    <div style={styles.container}>
      <div style={styles.header}>
        <h3>Rent Collection & Late Penalties</h3>
        <button style={styles.syncBtn}>Update All Statuses</button>
      </div>

      <table style={styles.table}>
        <thead>
          <tr style={styles.tableHead}>
            <th>Tenant</th>
            <th>Unit</th>
            <th>Monthly Rent</th>
            <th>Due Date</th>
            <th>Late Days</th>
            <th>Penalty (TZS)</th>
            <th>Status</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          {tenants.map(t => {
            const late = calculateLateInfo(t.dueDate, t.status);
            return (
              <tr key={t.id} style={{...styles.row, backgroundColor: late.days > 0 ? '#fff5f5' : '#fff'}}>
                <td>{t.name}</td>
                <td>{t.unit}</td>
                <td>{t.rent.toLocaleString()}</td>
                <td>{t.dueDate}</td>
                <td style={{color: 'red', fontWeight: 'bold'}}>{late.days > 0 ? late.days : '-'}</td>
                <td style={{color: 'red'}}>{late.penalty > 0 ? late.penalty.toLocaleString() : '-'}</td>
                <td>
                  <span style={{...styles.status, backgroundColor: t.status === 'Paid' ? '#27ae60' : '#e74c3c'}}>
                    {t.status}
                  </span>
                </td>
                <td>
                  <button style={styles.collectBtn}>Record Payment</button>
                </td>
              </tr>
            );
          })}
        </tbody>
      </table>
    </div>
  );
};const styles = {
  container: { padding: '25px', backgroundColor: '#fff', borderRadius: '15px', boxShadow: '0 4px 15px rgba(0,0,0,0.05)' },
  header: { display: 'flex', justifyContent: 'space-between', marginBottom: '20px' },
  syncBtn: { backgroundColor: '#34495e', color: '#fff', border: 'none', padding: '10px', borderRadius: '5px', cursor: 'pointer' },
  table: { width: '100%', borderCollapse: 'collapse' },
  tableHead: { textAlign: 'left', borderBottom: '2px solid #1a3a5f', paddingBottom: '10px' },
  row: { borderBottom: '1px solid #eee', transition: '0.3s' },
  status: { padding: '4px 10px', borderRadius: '20px', color: '#fff', fontSize: '12px' },
  collectBtn: { backgroundColor: '#1a3a5f', color: '#fff', border: 'none', padding: '6px 12px', borderRadius: '4px', cursor: 'pointer', fontSize: '12px' }
};import React, { useState } from 'react';

const MaintenanceModule = () => {
  const [repairs, setRepairs] = useState([
    { id: 101, property: 'Unit 4B - Sinza', issue: 'Broken Kitchen Pipe', fundi: 'Baba Juma (Plumber)', cost: 45000, status: 'Completed', date: '2026-03-22' },
    { id: 102, property: 'Mbezi Beach House', issue: 'AC Service', fundi: 'Technician Alex', cost: 120000, status: 'In Progress', date: '2026-03-24' },
    { id: 103, property: 'Unit 1A - Posta', issue: 'Electrical Sparking', fundi: 'Tanesco Certified Fundi', cost: 0, status: 'Pending', date: 'Today' }
  ]);

  return (
    <div style={styles.container}>
      <header style={styles.header}>
        <h3>Property Maintenance & Fundi Logs</h3>
        <button style={styles.reportBtn}>+ Report New Issue</button>
      </header>

      <div style={styles.grid}>
        {repairs.map((job) => (
          <div key={job.id} style={styles.repairCard}>
            <div style={styles.cardHeader}>
              <span style={styles.propTag}>{job.property}</span>
              <span style={{...styles.statusTag, backgroundColor: job.status === 'Completed' ? '#27ae60' : '#f1c40f'}}>
                {job.status}
              </span>
            </div>
            
            <h4>{job.issue}</h4>
            <p><strong>Assigned to:</strong> {job.fundi}</p>
            <p><strong>Cost:</strong> TZS {job.cost.toLocaleString()}</p>
            
            <div style={styles.photoBox}>
              <div style={styles.photoPlaceholder}>📷 Before Photo</div>
              <div style={styles.photoPlaceholder}>✅ After Photo</div>
            </div>

            <div style={styles.footer}>
              <small>Logged: {job.date}</small>
              <button style={styles.payBtn}>Record Payment</button>
            </div>
          </div>
        ))}
      </div>
    </div>
  );
};const styles = {
  container: { padding: '20px', backgroundColor: '#f9f9f9' },
  header: { display: 'flex', justifyContent: 'space-between', marginBottom: '25px' },
  reportBtn: { backgroundColor: '#e67e22', color: '#fff', border: 'none', padding: '12px 20px', borderRadius: '8px', cursor: 'pointer' },
  grid: { display: 'grid', gridTemplateColumns: 'repeat(auto-fit, minmax(320px, 1fr))', gap: '20px' },
  repairCard: { backgroundColor: '#fff', borderRadius: '12px', padding: '20px', boxShadow: '0 4px 6px rgba(0,0,0,0.05)', border: '1px solid #eee' },
  cardHeader: { display: 'flex', justifyContent: 'space-between', marginBottom: '10px' },
  propTag: { fontSize: '12px', fontWeight: 'bold', color: '#1a3a5f' },
  statusTag: { padding: '3px 8px', borderRadius: '4px', color: '#fff', fontSize: '11px' },
  photoBox: { display: 'flex', gap: '10px', margin: '15px 0' },
  photoPlaceholder: { flex: 1, height: '60px', backgroundColor: '#f0f2f5', border: '1px dashed #ccc', display: 'flex', alignItems: 'center', justifyContent: 'center', fontSize: '10px', color: '#888', borderRadius: '5px' },
  footer: { display: 'flex', justifyContent: 'space-between', alignItems: 'center', marginTop: '15px', borderTop: '1px solid #eee', paddingTop: '10px' },
  payBtn: { backgroundColor: '#1a3a5f', color: '#fff', border: 'none', padding: '5px 10px', borderRadius: '4px', cursor: 'pointer', fontSize: '12px' }
};import React from 'react';

const AnalyticsDashboard = () => {
  // Mock data representing the 5 divisions of Bigman Holdings
  const performanceData = [
    { division: 'Real Estate', revenue: 25400000, color: '#1a3a5f' },
    { division: 'Construction', revenue: 15200000, color: '#e67e22' },
    { division: 'Electronics', revenue: 8400000, color: '#27ae60' },
    { division: 'Car Dealership', revenue: 45000000, color: '#c0392b' },
    { division: 'IT Solutions', revenue: 3200000, color: '#8e44ad' }
  ];

  const totalRevenue = performanceData.reduce((acc, curr) => acc + curr.revenue, 0);

  return (
    <div style={styles.container}>
      <header style={styles.header}>
        <h3>Bigman Holdings: Performance Analytics</h3>
        <p>Reporting Period: March 2026</p>
      </header>

      {/* Top Level Stats */}
      <div style={styles.topStats}>
        <div style={styles.statCard}>
          <small>Total Holdings Revenue</small>
          <h2>TZS {totalRevenue.toLocaleString()}</h2>
          <span style={{color: 'green'}}>↑ 12% vs last month</span>
        </div>
        <div style={styles.statCard}>
          <small>Real Estate Vacancy Rate</small>
          <h2>14.5%</h2>
          <span style={{color: 'orange'}}>3 units empty</span>
        </div>
        <div style={styles.statCard}>
          <small>Pending Collections</small>
          <h2 style={{color: '#c0392b'}}>TZS 4,200,000</h2>
          <span>8 Tenants Overdue</span>
        </div>
      </div>

      {/* Division Ranking Chart (Visual representation using CSS) */}
      <div style={styles.chartSection}>
        <h4>Revenue Contribution by Division</h4>
        <div style={styles.barChart}>
          {performanceData.map(d => (
            <div key={d.division} style={styles.barWrapper}>
              <div 
                style={{
                  ...styles.bar, 
                  height: `${(d.revenue / 50000000) * 200}px`, 
                  backgroundColor: d.color
                }}
              ></div>
              <span style={styles.barLabel}>{d.division}</span>
              <small>{(d.revenue / 1000000).toFixed(1)}M</small>
            </div>
          ))}
        </div>
      </div>

      <button style={styles.exportBtn} onClick={() => alert('Generating Full Monthly PDF)
      </button>
    </div>
  );
};const styles = {
  container: { padding: '25px', backgroundColor: '#f4f7f6', minHeight: '100vh' },
  header: { marginBottom: '25px', borderBottom: '2px solid #1a3a5f' },
  topStats: { display: 'flex', gap: '20px', marginBottom: '30px', flexWrap: 'wrap' },
  statCard: { flex: 1, minWidth: '250px', backgroundColor: '#fff', padding: '20px', borderRadius: '15px', boxShadow: '0 4px 6px rgba(0,0,0,0.05)' },
  chartSection: { backgroundColor: '#fff', padding: '30px', borderRadius: '15px', marginBottom: '25px' },
  barChart: { display: 'flex', alignItems: 'flex-end', justifyContent: 'space-around', height: '250px', paddingTop: '20px' },
  barWrapper: { display: 'flex', flexDirection: 'column', alignItems: 'center', width: '60px' },
  bar: { width: '40px', borderRadius: '5px 5px 0 0', transition: 'height 0.5s ease' },
  barLabel: { fontSize: '12px', marginTop: '10px', fontWeight: 'bold', textAlign: 'center' },
  exportBtn: { width: '100%', padding: '15px', backgroundColor: '#1a3a5f', color: '#fff', border: 'none', borderRadius: '10px', cursor: 'pointer', fontWeight: 'bold' }
};
