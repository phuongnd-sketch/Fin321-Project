import React, { useState, useMemo } from 'react';
import { Download, Info, TrendingUp, DollarSign, Shield } from 'lucide-react';

const HedgeAnalysisModel = () => {
  // Input parameters (yellow section)
  const [inputs, setInputs] = useState({
    fcAmount: 1000000,
    fcCurrency: 'EUR',
    s0: 1.10,
    f0: 1.095,
    rUsd: 0.04,
    rFc: 0.02,
    strikePrice: 1.10,
    optionPremium: 0.02,
    optionType: 'put',
    timeToMaturity: 0.25
  });

  const updateInput = (key, value) => {
    setInputs(prev => ({ ...prev, [key]: parseFloat(value) || 0 }));
  };

  // Forward Hedge Calculation
  const forwardHedge = useMemo(() => {
    return inputs.fcAmount * inputs.f0;
  }, [inputs]);

  // Money Market Hedge Calculation
  const moneyMarketHedge = useMemo(() => {
    const pvFc = inputs.fcAmount / (1 + inputs.rFc * inputs.timeToMaturity);
    const usdConverted = pvFc * inputs.s0;
    const fvUsd = usdConverted * (1 + inputs.rUsd * inputs.timeToMaturity);
    return {
      pvFc,
      usdConverted,
      fvUsd,
      steps: [
        { step: '1. Borrow PV of FC', value: pvFc, formula: `${inputs.fcAmount} / (1 + ${inputs.rFc} × ${inputs.timeToMaturity})` },
        { step: '2. Convert to USD at S₀', value: usdConverted, formula: `${pvFc.toFixed(2)} × ${inputs.s0}` },
        { step: '3. Invest USD to maturity', value: fvUsd, formula: `${usdConverted.toFixed(2)} × (1 + ${inputs.rUsd} × ${inputs.timeToMaturity})` }
      ]
    };
  }, [inputs]);

  // Option Hedge Calculation
  const calculateOptionProceeds = (sT) => {
    const premiumCost = inputs.fcAmount * inputs.optionPremium;
    const premiumFV = premiumCost * (1 + inputs.rUsd * inputs.timeToMaturity);
    
    let proceeds;
    if (inputs.optionType === 'put') {
      // Put: protect against FC appreciation (USD depreciation)
      const spotProceeds = inputs.fcAmount * sT;
      const optionPayoff = Math.max(0, inputs.fcAmount * (inputs.strikePrice - sT));
      proceeds = spotProceeds + optionPayoff - premiumFV;
    } else {
      // Call: protect against FC depreciation (USD appreciation)
      const spotProceeds = inputs.fcAmount * sT;
      const optionPayoff = Math.max(0, inputs.fcAmount * (sT - inputs.strikePrice));
      proceeds = spotProceeds + optionPayoff - premiumFV;
    }
    
    return { proceeds, premiumFV };
  };

  // Sensitivity Analysis (±5% in 1% increments)
  const sensitivityData = useMemo(() => {
    const baseRate = inputs.s0;
    const scenarios = [];
    
    for (let pct = -5; pct <= 5; pct += 1) {
      const sT = baseRate * (1 + pct / 100);
      const unhedged = inputs.fcAmount * sT;
      const forward = forwardHedge;
      const mm = moneyMarketHedge.fvUsd;
      const option = calculateOptionProceeds(sT);
      
      scenarios.push({
        pct,
        sT: sT.toFixed(4),
        unhedged,
        forward,
        mm,
        option: option.proceeds
      });
    }
    
    return scenarios;
  }, [inputs, forwardHedge, moneyMarketHedge]);

  // Interest Rate Parity Check
  const parityCheck = useMemo(() => {
    const theoreticalForward = inputs.s0 * (1 + inputs.rUsd * inputs.timeToMaturity) / 
                                (1 + inputs.rFc * inputs.timeToMaturity);
    const difference = Math.abs(inputs.f0 - theoreticalForward);
    const pctDiff = (difference / theoreticalForward) * 100;
    return {
      theoretical: theoreticalForward,
      actual: inputs.f0,
      difference,
      pctDiff,
      satisfied: pctDiff < 0.5
    };
  }, [inputs]);

  const downloadExcel = () => {
    alert('Excel download functionality would generate an .xlsx file with:\n\n' +
          '• Inputs (yellow highlighted)\n' +
          '• Forward hedge calculation\n' +
          '• Money market hedge with steps\n' +
          '• Option hedge analysis\n' +
          '• Sensitivity table (±5%)\n' +
          '• Summary comparison\n' +
          '• Charts and documentation\n\n' +
          'For actual implementation, copy the calculations into Excel following the structure shown.');
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-slate-50 to-slate-100 p-6">
      <div className="max-w-7xl mx-auto space-y-6">
        
        {/* Header */}
        <div className="bg-white rounded-xl shadow-lg p-6 border-t-4 border-blue-600">
          <div className="flex items-center justify-between">
            <div>
              <h1 className="text-3xl font-bold text-slate-800 flex items-center gap-3">
                <Shield className="text-blue-600" size={32} />
                FX Hedge Analysis Model
              </h1>
              <p className="text-slate-600 mt-2">Stage 3: Excel Model Build - FIN 321</p>
            </div>
            <button
              onClick={downloadExcel}
              className="flex items-center gap-2 bg-green-600 text-white px-6 py-3 rounded-lg hover:bg-green-700 transition-colors font-semibold"
            >
              <Download size={20} />
              Export to Excel
            </button>
          </div>
        </div>

        {/* Inputs Section (Yellow) */}
        <div className="bg-yellow-50 rounded-xl shadow-lg p-6 border-2 border-yellow-300">
          <h2 className="text-2xl font-bold text-slate-800 mb-4 flex items-center gap-2">
            <DollarSign className="text-yellow-600" />
            Input Parameters
          </h2>
          
          <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
            <div>
              <label className="block text-sm font-semibold text-slate-700 mb-1">
                Foreign Currency Amount
              </label>
              <input
                type="number"
                value={inputs.fcAmount}
                onChange={(e) => updateInput('fcAmount', e.target.value)}
                className="w-full px-4 py-2 border-2 border-yellow-400 rounded-lg bg-yellow-100 focus:ring-2 focus:ring-yellow-500"
              />
            </div>
            
            <div>
              <label className="block text-sm font-semibold text-slate-700 mb-1">
                Currency
              </label>
              <select
                value={inputs.fcCurrency}
                onChange={(e) => setInputs(prev => ({ ...prev, fcCurrency: e.target.value }))}
                className="w-full px-4 py-2 border-2 border-yellow-400 rounded-lg bg-yellow-100 focus:ring-2 focus:ring-yellow-500"
              >
                <option>EUR</option>
                <option>GBP</option>
                <option>JPY</option>
                <option>CHF</option>
              </select>
            </div>
            
            <div>
              <label className="block text-sm font-semibold text-slate-700 mb-1">
                Spot Rate (S₀)
              </label>
              <input
                type="number"
                step="0.0001"
                value={inputs.s0}
                onChange={(e) => updateInput('s0', e.target.value)}
                className="w-full px-4 py-2 border-2 border-yellow-400 rounded-lg bg-yellow-100 focus:ring-2 focus:ring-yellow-500"
              />
            </div>
            
            <div>
              <label className="block text-sm font-semibold text-slate-700 mb-1">
                Forward Rate (F₀)
              </label>
              <input
                type="number"
                step="0.0001"
                value={inputs.f0}
                onChange={(e) => updateInput('f0', e.target.value)}
                className="w-full px-4 py-2 border-2 border-yellow-400 rounded-lg bg-yellow-100 focus:ring-2 focus:ring-yellow-500"
              />
            </div>
            
            <div>
              <label className="block text-sm font-semibold text-slate-700 mb-1">
                USD Interest Rate (r_USD)
              </label>
              <input
                type="number"
                step="0.001"
                value={inputs.rUsd}
                onChange={(e) => updateInput('rUsd', e.target.value)}
                className="w-full px-4 py-2 border-2 border-yellow-400 rounded-lg bg-yellow-100 focus:ring-2 focus:ring-yellow-500"
              />
            </div>
            
            <div>
              <label className="block text-sm font-semibold text-slate-700 mb-1">
                FC Interest Rate (r_FC)
              </label>
              <input
                type="number"
                step="0.001"
                value={inputs.rFc}
                onChange={(e) => updateInput('rFc', e.target.value)}
                className="w-full px-4 py-2 border-2 border-yellow-400 rounded-lg bg-yellow-100 focus:ring-2 focus:ring-yellow-500"
              />
            </div>
            
            <div>
              <label className="block text-sm font-semibold text-slate-700 mb-1">
                Strike Price (K)
              </label>
              <input
                type="number"
                step="0.0001"
                value={inputs.strikePrice}
                onChange={(e) => updateInput('strikePrice', e.target.value)}
                className="w-full px-4 py-2 border-2 border-yellow-400 rounded-lg bg-yellow-100 focus:ring-2 focus:ring-yellow-500"
              />
            </div>
            
            <div>
              <label className="block text-sm font-semibold text-slate-700 mb-1">
                Option Premium (per unit)
              </label>
              <input
                type="number"
                step="0.001"
                value={inputs.optionPremium}
                onChange={(e) => updateInput('optionPremium', e.target.value)}
                className="w-full px-4 py-2 border-2 border-yellow-400 rounded-lg bg-yellow-100 focus:ring-2 focus:ring-yellow-500"
              />
            </div>
            
            <div>
              <label className="block text-sm font-semibold text-slate-700 mb-1">
                Option Type
              </label>
              <select
                value={inputs.optionType}
                onChange={(e) => setInputs(prev => ({ ...prev, optionType: e.target.value }))}
                className="w-full px-4 py-2 border-2 border-yellow-400 rounded-lg bg-yellow-100 focus:ring-2 focus:ring-yellow-500"
              >
                <option value="put">Put Option</option>
                <option value="call">Call Option</option>
              </select>
            </div>
            
            <div>
              <label className="block text-sm font-semibold text-slate-700 mb-1">
                Time to Maturity (years)
              </label>
              <input
                type="number"
                step="0.01"
                value={inputs.timeToMaturity}
                onChange={(e) => updateInput('timeToMaturity', e.target.value)}
                className="w-full px-4 py-2 border-2 border-yellow-400 rounded-lg bg-yellow-100 focus:ring-2 focus:ring-yellow-500"
              />
            </div>
          </div>
        </div>

        {/* Hedge Calculations */}
        <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
          
          {/* Forward Hedge */}
          <div className="bg-white rounded-xl shadow-lg p-6">
            <h3 className="text-xl font-bold text-slate-800 mb-4 flex items-center gap-2">
              <TrendingUp className="text-blue-600" />
              Forward Hedge
            </h3>
            <div className="space-y-3">
              <div className="bg-blue-50 p-4 rounded-lg">
                <div className="text-sm text-slate-600 mb-1">Formula</div>
                <div className="font-mono text-sm text-slate-700">
                  USD Proceeds = FC_AMT × F₀
                </div>
              </div>
              <div className="bg-blue-50 p-4 rounded-lg">
                <div className="text-sm text-slate-600 mb-1">Calculation</div>
                <div className="font-mono text-sm text-slate-700">
                  = {inputs.fcAmount.toLocaleString()} × {inputs.f0}
                </div>
              </div>
              <div className="bg-blue-600 text-white p-4 rounded-lg">
                <div className="text-sm mb-1">USD Proceeds (Locked In)</div>
                <div className="text-2xl font-bold">
                  ${forwardHedge.toLocaleString('en-US', { minimumFractionDigits: 2, maximumFractionDigits: 2 })}
                </div>
              </div>
            </div>
          </div>

          {/* Money Market Hedge */}
          <div className="bg-white rounded-xl shadow-lg p-6">
            <h3 className="text-xl font-bold text-slate-800 mb-4 flex items-center gap-2">
              <DollarSign className="text-green-600" />
              Money Market Hedge
            </h3>
            <div className="space-y-3">
              {moneyMarketHedge.steps.map((step, idx) => (
                <div key={idx} className="bg-green-50 p-3 rounded-lg">
                  <div className="text-sm font-semibold text-slate-700">{step.step}</div>
                  <div className="font-mono text-xs text-slate-600 mt-1">{step.formula}</div>
                  <div className="text-lg font-bold text-green-700 mt-1">
                    {step.value.toLocaleString('en-US', { minimumFractionDigits: 2, maximumFractionDigits: 2 })}
                  </div>
                </div>
              ))}
              <div className="bg-green-600 text-white p-4 rounded-lg">
                <div className="text-sm mb-1">Final USD Proceeds</div>
                <div className="text-2xl font-bold">
                  ${moneyMarketHedge.fvUsd.toLocaleString('en-US', { minimumFractionDigits: 2, maximumFractionDigits: 2 })}
                </div>
              </div>
            </div>
          </div>
        </div>

        {/* Interest Rate Parity Check */}
        <div className="bg-white rounded-xl shadow-lg p-6">
          <h3 className="text-xl font-bold text-slate-800 mb-4 flex items-center gap-2">
            <Info className="text-purple-600" />
            Interest Rate Parity Check
          </h3>
          <div className="grid grid-cols-1 md:grid-cols-4 gap-4">
            <div className="bg-purple-50 p-4 rounded-lg">
              <div className="text-sm text-slate-600 mb-1">Theoretical F₀</div>
              <div className="text-xl font-bold text-purple-700">
                {parityCheck.theoretical.toFixed(4)}
              </div>
            </div>
            <div className="bg-purple-50 p-4 rounded-lg">
              <div className="text-sm text-slate-600 mb-1">Actual F₀</div>
              <div className="text-xl font-bold text-purple-700">
                {parityCheck.actual.toFixed(4)}
              </div>
            </div>
            <div className="bg-purple-50 p-4 rounded-lg">
              <div className="text-sm text-slate-600 mb-1">Difference</div>
              <div className="text-xl font-bold text-purple-700">
                {parityCheck.difference.toFixed(4)}
              </div>
            </div>
            <div className={`p-4 rounded-lg ${parityCheck.satisfied ? 'bg-green-100' : 'bg-red-100'}`}>
              <div className="text-sm text-slate-600 mb-1">Status</div>
              <div className={`text-xl font-bold ${parityCheck.satisfied ? 'text-green-700' : 'text-red-700'}`}>
                {parityCheck.satisfied ? '✓ Satisfied' : '✗ Violation'}
              </div>
              <div className="text-xs text-slate-600 mt-1">
                ({parityCheck.pctDiff.toFixed(2)}%)
              </div>
            </div>
          </div>
        </div>

        {/* Sensitivity Analysis */}
        <div className="bg-white rounded-xl shadow-lg p-6">
          <h3 className="text-xl font-bold text-slate-800 mb-4">
            Sensitivity Analysis (±5% Spot Rate Movement)
          </h3>
          <div className="overflow-x-auto">
            <table className="w-full text-sm">
              <thead>
                <tr className="bg-slate-100">
                  <th className="p-3 text-left font-semibold">Scenario</th>
                  <th className="p-3 text-right font-semibold">S_T</th>
                  <th className="p-3 text-right font-semibold">Unhedged</th>
                  <th className="p-3 text-right font-semibold">Forward</th>
                  <th className="p-3 text-right font-semibold">Money Mkt</th>
                  <th className="p-3 text-right font-semibold">Option</th>
                </tr>
              </thead>
              <tbody>
                {sensitivityData.map((row, idx) => (
                  <tr key={idx} className={idx % 2 === 0 ? 'bg-slate-50' : 'bg-white'}>
                    <td className="p-3 font-semibold">{row.pct > 0 ? '+' : ''}{row.pct}%</td>
                    <td className="p-3 text-right font-mono">{row.sT}</td>
                    <td className="p-3 text-right">${row.unhedged.toLocaleString('en-US', {maximumFractionDigits: 0})}</td>
                    <td className="p-3 text-right">${row.forward.toLocaleString('en-US', {maximumFractionDigits: 0})}</td>
                    <td className="p-3 text-right">${row.mm.toLocaleString('en-US', {maximumFractionDigits: 0})}</td>
                    <td className="p-3 text-right">${row.option.toLocaleString('en-US', {maximumFractionDigits: 0})}</td>
                  </tr>
                ))}
              </tbody>
            </table>
          </div>
        </div>

        {/* Summary & Recommendation */}
        <div className="bg-gradient-to-r from-blue-600 to-purple-600 rounded-xl shadow-lg p-6 text-white">
          <h3 className="text-2xl font-bold mb-4">Summary & Output</h3>
          <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
            <div className="bg-white/10 backdrop-blur p-4 rounded-lg">
              <div className="text-sm mb-1">Forward Hedge Result</div>
              <div className="text-2xl font-bold">
                ${forwardHedge.toLocaleString('en-US', {maximumFractionDigits: 0})}
              </div>
            </div>
            <div className="bg-white/10 backdrop-blur p-4 rounded-lg">
              <div className="text-sm mb-1">Money Market Result</div>
              <div className="text-2xl font-bold">
                ${moneyMarketHedge.fvUsd.toLocaleString('en-US', {maximumFractionDigits: 0})}
              </div>
            </div>
            <div className="bg-white/10 backdrop-blur p-4 rounded-lg">
              <div className="text-sm mb-1">Parity Check</div>
              <div className="text-2xl font-bold">
                {Math.abs(forwardHedge - moneyMarketHedge.fvUsd) < 100 ? '✓ Pass' : '⚠ Review'}
              </div>
            </div>
          </div>
          <div className="mt-4 p-4 bg-white/10 backdrop-blur rounded-lg">
            <div className="text-sm mb-2">Recommendation Placeholder</div>
            <div className="text-base italic">
              To be finalized in Stage 5 based on risk tolerance and market outlook.
            </div>
          </div>
        </div>

        {/* Notes & Documentation */}
        <div className="bg-slate-800 text-white rounded-xl shadow-lg p-6">
          <h3 className="text-xl font-bold mb-4">Model Notes & Assumptions</h3>
          <ul className="space-y-2 text-sm">
            <li>• All interest rates are annualized and compounded using simple interest</li>
            <li>• Option premiums are paid upfront and compounded to maturity</li>
            <li>• Forward and Money Market hedges should yield nearly identical results (IRP)</li>
            <li>• Sensitivity analysis uses ±5% range in 1% increments</li>
            <li>• Put option protects against FC appreciation (receivable scenario)</li>
            <li>• All calculations assume no transaction costs or bid-ask spreads</li>
            <li>• Model uses named ranges for key cells (FC_AMT, S0, F0, etc.)</li>
          </ul>
        </div>

      </div>
    </div>
  );
};

export default HedgeAnalysisModel;
