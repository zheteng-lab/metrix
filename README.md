import React, { useState } from 'react';
import { Target, TrendingUp, Users, RefreshCw, Star, ClipboardCheck, DollarSign, MousePointerClick, Activity } from 'lucide-react';

const App = () => {
  const [data, setData] = useState({
    // PRIMARY FUNNEL (The "Power Trio")
    leads: { target: "450 Raw / 113 Quality", actual: "", status: "yellow", priority: true },
    conversion: { target: "40% Lead-to-Trial", actual: "", status: "yellow", priority: true },
    enrollment: { target: "18 Students/mo", actual: "", status: "green", priority: true },
    
    // FINANCIAL & SCALE
    revenue: { target: "RM 73k/mo", actual: "73,000", status: "green" },
    mf_growth: { target: "RM 10k/franchisee", actual: "", status: "green" },
    utilization: { target: "75% Occupancy", actual: "", status: "green" },
    
    // RETENTION & QUALITY
    churn: { target: "< 4 students/mo", actual: "", status: "green" },
    compliance: { target: "100% SOP", actual: "", status: "green" },
    referrals: { target: "2-3 Monthly", actual: "", status: "green" },
  });

  const toggleStatus = (key) => {
    const states = ['green', 'yellow', 'red'];
    setData(prev => ({
      ...prev,
      [key]: { ...prev[key], status: states[(states.indexOf(prev[key].status) + 1) % 3] }
    }));
  };

  const updateActual = (key, val) => {
    setData(prev => ({
      ...prev,
      [key]: { ...prev[key], actual: val }
    }));
  };

  const StatusLight = ({ status, onClick }) => {
    const colors = {
      green: 'bg-green-500 shadow-[0_0_8px_rgba(34,197,94,0.4)]',
      yellow: 'bg-yellow-500 shadow-[0_0_8px_rgba(234,179,8,0.4)]',
      red: 'bg-red-500 shadow-[0_0_8px_rgba(239,68,68,0.4)]'
    };
    return (
      <div 
        onClick={onClick}
        className={`w-4 h-4 rounded-full cursor-pointer transition-all duration-300 ${colors[status]}`}
      />
    );
  };

  const Card = ({ title, id, icon: Icon, target, actual, status, isPriority }) => (
    <div className={`p-5 rounded-xl border transition-all duration-300 flex flex-col justify-between h-48 ${
      isPriority 
      ? "bg-white border-blue-200 shadow-md ring-2 ring-blue-50/50" 
      : "bg-slate-50/50 border-slate-200 opacity-90 shadow-sm"
    }`}>
      <div className="flex justify-between items-start">
        <div className={`p-2 rounded-lg ${isPriority ? "bg-blue-100 text-blue-600" : "bg-slate-100 text-slate-500"}`}>
          <Icon className="w-5 h-5" />
        </div>
        <div className="flex items-center gap-2">
          {isPriority && <span className="text-[10px] font-bold text-blue-500 uppercase tracking-tighter">Core Metric</span>}
          <StatusLight status={status} onClick={() => toggleStatus(id)} />
        </div>
      </div>
      
      <div className="mt-3">
        <h3 className={`text-sm font-bold uppercase tracking-wider ${isPriority ? "text-slate-800" : "text-slate-500"}`}>
          {title}
        </h3>
        <p className="text-xs text-slate-400 mt-1 italic">Target: {target}</p>
      </div>

      <div className="mt-4">
        <label className="block text-[10px] text-slate-400 uppercase font-bold mb-1">Monthly Actual</label>
        <input 
          type="text" 
          value={actual}
          onChange={(e) => updateActual(id, e.target.value)}
          placeholder="0"
          className="w-full bg-white border border-slate-200 rounded px-2 py-1 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500/20"
        />
      </div>
    </div>
  );

  return (
    <div className="min-h-screen bg-white p-4 md:p-8 font-sans">
      <div className="max-w-5xl mx-auto">
        <header className="mb-8 flex flex-col md:flex-row md:items-end justify-between gap-4 border-b border-slate-100 pb-6">
          <div>
            <h1 className="text-3xl font-black text-slate-900 tracking-tight">Management Dashboard</h1>
            <p className="text-slate-500 font-medium">Classone Online Core Growth Matrix</p>
          </div>
          <div className="flex gap-3">
            {['green', 'yellow', 'red'].map(s => (
              <div key={s} className="flex items-center gap-2 bg-slate-50 px-3 py-1.5 rounded-full border border-slate-100 text-[11px] font-bold text-slate-600 uppercase">
                <div className={`w-2 h-2 rounded-full ${s === 'green' ? 'bg-green-500' : s === 'yellow' ? 'bg-yellow-500' : 'bg-red-500'}`}></div>
                {s === 'green' ? 'Healthy' : s === 'yellow' ? 'Warning' : 'Critical'}
              </div>
            ))}
          </div>
        </header>

        {/* 3x3 Grid Reorganized */}
        <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
          {/* COLUMN 1: TOP FUNNEL */}
          <Card 
            title="Lead Volume" 
            id="leads"
            icon={MousePointerClick}
            target={data.leads.target}
            actual={data.leads.actual}
            status={data.leads.status}
            isPriority={true}
          />
          <Card 
            title="Revenue Achievement" 
            id="revenue"
            icon={DollarSign}
            target={data.revenue.target}
            actual={data.revenue.actual}
            status={data.revenue.status}
          />
          <Card 
            title="Student Churn" 
            id="churn"
            icon={RefreshCw}
            target={data.churn.target}
            actual={data.churn.actual}
            status={data.churn.status}
          />

          {/* COLUMN 2: CONVERSION & OPS */}
          <Card 
            title="Trial Conversion" 
            id="conversion"
            icon={TrendingUp}
            target={data.conversion.target}
            actual={data.conversion.actual}
            status={data.conversion.status}
            isPriority={true}
          />
          <Card 
            title="Teacher Utilization" 
            id="utilization"
            icon={Activity}
            target={data.utilization.target}
            actual={data.utilization.actual}
            status={data.utilization.status}
          />
          <Card 
            title="SOP Compliance" 
            id="compliance"
            icon={ClipboardCheck}
            target={data.compliance.target}
            actual={data.compliance.actual}
            status={data.compliance.status}
          />

          {/* COLUMN 3: RESULT & REPUTATION */}
          <Card 
            title="New Enrollments" 
            id="enrollment"
            icon={Users}
            target={data.enrollment.target}
            actual={data.enrollment.actual}
            status={data.enrollment.status}
            isPriority={true}
          />
          <Card 
            title="MF Contribution" 
            id="mf_growth"
            icon={Target}
            target={data.mf_growth.target}
            actual={data.mf_growth.actual}
            status={data.mf_growth.status}
          />
          <Card 
            title="Referral Count" 
            id="referrals"
            icon={Star}
            target={data.referrals.target}
            actual={data.referrals.actual}
            status={data.referrals.status}
          />
        </div>

        <div className="mt-8 grid grid-cols-1 md:grid-cols-3 gap-6">
           <div className="md:col-span-2 p-6 bg-slate-900 rounded-2xl text-white shadow-xl">
              <h2 className="text-xl font-bold mb-4 flex items-center gap-2">
                <Activity className="text-blue-400" /> Executive Summary
              </h2>
              <div className="grid grid-cols-2 gap-8">
                <div>
                  <p className="text-slate-400 text-xs uppercase font-black mb-2">Primary Bottleneck</p>
                  <p className="text-sm leading-relaxed">Marketing lead volume is currently insufficient, creating a ripple effect that prevents the sales team from hitting enrollment targets despite healthy trial interest.</p>
                </div>
                <div>
                  <p className="text-slate-400 text-xs uppercase font-black mb-2">Immediate Focus</p>
                  <p className="text-sm leading-relaxed text-blue-300 font-medium">Optimize Lead Quality vs. Quantity and standardize the trial-to-close sales script training at the center level.</p>
                </div>
              </div>
           </div>
           <div className="p-6 bg-blue-600 rounded-2xl text-white shadow-xl flex flex-col justify-center text-center">
              <p className="text-blue-100 text-xs uppercase font-black mb-1">Monthly Goal</p>
              <h3 className="text-3xl font-black">18</h3>
              <p className="text-sm font-bold">New Students</p>
           </div>
        </div>
      </div>
    </div>
  );
};

export default App;
