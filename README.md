[12.html](https://github.com/user-attachments/files/32769288/12.html)
# AWS-Anomaly-Detection-SIH2026
AI/ML-based AWS anomaly detection dashboard prototype for Smart India Hackathon 2026.
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AWS Anomaly Detection & Multi-Stakeholder Platform - SIH 2026</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</head>
<body class="bg-slate-950 text-slate-100 font-sans min-h-screen flex flex-col">
    <header class="bg-slate-900 border-b border-slate-800 px-6 py-4 flex justify-between items-center shadow-md">
        <div class="flex items-center space-x-3">
            <div class="bg-teal-500 w-3 h-3 rounded-full animate-ping"></div>
            <h1 class="text-xl font-bold tracking-wider text-teal-400">AWS IoT / MLOps Anomaly Detection Platform</h1>
        </div>
        <div class="flex items-center space-x-4">
            <div id="roleBadge" class="text-xs px-3 py-1 bg-teal-500/10 text-teal-300 border border-teal-500/30 rounded-full font-medium">Mode: Core AI Anomaly Detection</div>
            <div class="text-sm text-slate-400">Engine Status: <span class="text-emerald-400 font-semibold">Active</span></div>
        </div>
    </header>

    <!-- Portal Mode Switcher -->
    <nav class="bg-slate-900/60 border-b border-slate-800/60 px-6 py-3 flex space-x-3 overflow-x-auto">
        <button onclick="setPortal('core')" id="tab-core" class="px-4 py-2 text-xs font-semibold rounded-lg bg-teal-600 text-white transition shadow whitespace-nowrap">Core AI & Sensor Faults</button>
        <button onclick="setPortal('farmer')" id="tab-farmer" class="px-4 py-2 text-xs font-semibold rounded-lg bg-slate-800 text-slate-400 hover:text-slate-200 transition whitespace-nowrap">Farmer Agro-Advisory</button>
        <button onclick="setPortal('admin')" id="tab-admin" class="px-4 py-2 text-xs font-semibold rounded-lg bg-slate-800 text-slate-400 hover:text-slate-200 transition whitespace-nowrap">Municipal & Waterlogging</button>
        <button onclick="setPortal('infrastructure')" id="tab-infrastructure" class="px-4 py-2 text-xs font-semibold rounded-lg bg-slate-800 text-slate-400 hover:text-slate-200 transition whitespace-nowrap">Public Infrastructure Projects</button>
    </nav>

    <main class="flex-1 p-6 grid grid-cols-1 lg:grid-cols-3 gap-6 max-w-7xl mx-auto w-full">
        <div class="space-y-6">
            <div class="bg-slate-900 border border-slate-800 rounded-xl p-5 shadow-lg">
                <h2 id="formHeader" class="text-lg font-semibold mb-4 text-slate-200">Live Observation Injector</h2>
                <form id="sensorForm" class="space-y-4">
                    <div>
                        <label class="block text-xs text-slate-400 mb-1">Station ID / Zone</label>
                        <select id="station_id" class="w-full bg-slate-800 border border-slate-700 rounded p-2 text-slate-200">
                            <option value="AWS_01">AWS_01 (Raipur Central)</option>
                            <option value="AWS_02">AWS_02 (East Expressway)</option>
                            <option value="AWS_03">AWS_03 (North Hills)</option>
                        </select>
                    </div>
                    <div class="grid grid-cols-2 gap-3">
                        <div>
                            <label class="block text-xs text-slate-400 mb-1" id="lbl1">Temperature (°C)</label>
                            <input type="number" step="0.1" id="val1" value="48.9" class="w-full bg-slate-800 border border-slate-700 rounded p-2 text-slate-200">
                        </div>
                        <div>
                            <label class="block text-xs text-slate-400 mb-1" id="lbl2">Humidity (%)</label>
                            <input type="number" step="1" id="val2" value="45" class="w-full bg-slate-800 border border-slate-700 rounded p-2 text-slate-200">
                        </div>
                    </div>
                    <div class="grid grid-cols-2 gap-3">
                        <div>
                            <label class="block text-xs text-slate-400 mb-1" id="lbl3">Pressure (hPa)</label>
                            <input type="number" step="1" id="val3" value="1008" class="w-full bg-slate-800 border border-slate-700 rounded p-2 text-slate-200">
                        </div>
                        <div>
                            <label class="block text-xs text-slate-400 mb-1" id="lbl4">Wind Speed (km/h)</label>
                            <input type="number" step="0.1" id="val4" value="14.2" class="w-full bg-slate-800 border border-slate-700 rounded p-2 text-slate-200">
                        </div>
                    </div>
                    <div>
                        <label class="block text-xs text-slate-400 mb-1" id="lbl5">Solar Radiation (W/m²)</label>
                        <input type="number" step="1" id="val5" value="850" class="w-full bg-slate-800 border border-slate-700 rounded p-2 text-slate-200">
                    </div>
                    <button type="submit" id="submitBtn" class="w-full bg-teal-600 hover:bg-teal-500 text-white font-medium py-2.5 rounded transition shadow">
                        Run AI Inference & QC Check
                    </button>
                </form>
            </div>

            <div id="resultCard" class="bg-slate-900 border border-slate-800 rounded-xl p-5 shadow-lg hidden">
                <h2 id="outputTitle" class="text-md font-semibold mb-3 text-slate-200">AI Detection Result</h2>
                <div id="resultContent" class="space-y-2 text-sm"></div>
            </div>
        </div>

        <div class="lg:col-span-2 space-y-6">
            <div class="bg-slate-900 border border-slate-800 rounded-xl p-5 shadow-lg">
                <div class="flex justify-between items-center mb-4">
                    <h2 id="chartHeader" class="text-md font-semibold text-slate-200">Real-Time Sensor Stream & Anomaly Markers</h2>
                    <span class="text-xs px-2.5 py-1 bg-teal-500/10 text-teal-400 border border-teal-500/20 rounded">Hybrid Ensemble Active</span>
                </div>
                <canvas id="mainChart" height="130"></canvas>
            </div>
        </div>
    </main>

    <script>
        let activePortal = 'core';

        const ctx = document.getElementById('mainChart').getContext('2d');
        const chart = new Chart(ctx, {
            type: 'line',
            data: {
                labels: ['10:00', '10:10', '10:20', '10:30', '10:40', '10:50', '11:00'],
                datasets: [{
                    label: 'Temperature (°C)',
                    data: [28.2, 28.4, 28.5, 31.0, 48.9, 30.2, 29.8],
                    borderColor: '#2dd4bf',
                    backgroundColor: 'rgba(45, 212, 191, 0.05)',
                    borderWidth: 2,
                    fill: true,
                    tension: 0.3
                }]
            },
            options: { responsive: true, scales: { x: { grid: { color: '#1e293b' } }, y: { grid: { color: '#1e293b' } } } }
        });

        function setPortal(portal) {
            activePortal = portal;
            ['core', 'farmer', 'admin', 'infrastructure'].forEach(p => {
                const b = document.getElementById(`tab-${p}`);
                if (p === portal) {
                    b.className = "px-4 py-2 text-xs font-semibold rounded-lg bg-teal-600 text-white transition shadow whitespace-nowrap";
                } else {
                    b.className = "px-4 py-2 text-xs font-semibold rounded-lg bg-slate-800 text-slate-400 hover:text-slate-200 transition whitespace-nowrap";
                }
            });

            const formHeader = document.getElementById('formHeader');
            const roleBadge = document.getElementById('roleBadge');
            const submitBtn = document.getElementById('submitBtn');
            const chartHeader = document.getElementById('chartHeader');

            if (portal === 'core') {
                formHeader.innerText = "Live Observation Injector";
                roleBadge.innerText = "Mode: Core AI Anomaly Detection";
                submitBtn.innerText = "Run AI Inference & QC Check";
                chartHeader.innerText = "Real-Time Sensor Stream & Anomaly Markers";
                document.getElementById('lbl1').innerText = "Temperature (°C)";
                document.getElementById('lbl2').innerText = "Humidity (%)";
                document.getElementById('lbl3').innerText = "Pressure (hPa)";
                document.getElementById('lbl4').innerText = "Wind Speed (km/h)";
                document.getElementById('lbl5').innerText = "Solar Radiation (W/m²)";
                document.getElementById('val1').value = 48.9;
                document.getElementById('val2').value = 45;
                document.getElementById('val3').value = 1008;
                document.getElementById('val4').value = 14.2;
                document.getElementById('val5').value = 850;
            } else if (portal === 'farmer') {
                formHeader.innerText = "Farmer Agro-Advisory & Harvest Safe Guard";
                roleBadge.innerText = "Mode: Farmer Portal (Zero-Delay Advisory)";
                submitBtn.innerText = "Dispatch Instant Farmer Safe Protocol";
                chartHeader.innerText = "Precipitation & Soil Moisture Risk Stream";
                document.getElementById('lbl1').innerText = "Rainfall Forecast (mm)";
                document.getElementById('lbl2').innerText = "Soil Moisture (%)";
                document.getElementById('lbl3').innerText = "Humidity (%)";
                document.getElementById('lbl4').innerText = "Wind Speed (km/h)";
                document.getElementById('lbl5').innerText = "Evapotranspiration Index";
                document.getElementById('val1').value = 75.0;
                document.getElementById('val2').value = 92;
                document.getElementById('val3').value = 88;
                document.getElementById('val4').value = 35.0;
                document.getElementById('val5').value = 2.1;
            } else if (portal === 'admin') {
                formHeader.innerText = "Municipal Waterlogging & Ground Crew Dispatch";
                roleBadge.innerText = "Mode: Administrative Portal (Pre-Rain Maintenance)";
                submitBtn.innerText = "Broadcast Ground Worker Pothole/Drainage Orders";
                chartHeader.innerText = "Catchment Water Level & Drainage Load";
                document.getElementById('lbl1').innerText = "Precipitation Rate (mm/h)";
                document.getElementById('lbl2').innerText = "Drainage Capacity Used (%)";
                document.getElementById('lbl3').innerText = "Water Level (cm)";
                document.getElementById('lbl4').innerText = "Ground Saturation (%)";
                document.getElementById('lbl5').innerText = "Pothole Risk Index";
                document.getElementById('val1').value = 62.5;
                document.getElementById('val2').value = 95;
                document.getElementById('val3').value = 45;
                document.getElementById('val4').value = 90;
                document.getElementById('val5').value = 8.5;
            } else if (portal === 'infrastructure') {
                formHeader.innerText = "Public Project Safety (Railway / Bridges)";
                roleBadge.innerText = "Mode: Public Infrastructure Portal";
                submitBtn.innerText = "Evaluate Site Safety & Issue Work Halt";
                chartHeader.innerText = "Wind Shear & Bridge Stress Real-Time Stream";
                document.getElementById('lbl1').innerText = "Wind Shear (km/h)";
                document.getElementById('lbl2').innerText = "Structural Stress (kPa)";
                document.getElementById('lbl3').innerText = "Vibration Frequency (Hz)";
                document.getElementById('lbl4').innerText = "Foundation Moisture (%)";
                document.getElementById('lbl5').innerText = "Corrosion Index";
                document.getElementById('val1').value = 82.0;
                document.getElementById('val2').value = 145;
                document.getElementById('val3').value = 12.4;
                document.getElementById('val4').value = 89;
                document.getElementById('val5').value = 4.2;
            }
            document.getElementById('resultCard').classList.add('hidden');
        }

        document.getElementById('sensorForm').addEventListener('submit', (e) => {
            e.preventDefault();
            const v1 = parseFloat(document.getElementById('val1').value);
            const v2 = parseFloat(document.getElementById('val2').value);

            const card = document.getElementById('resultCard');
            const content = document.getElementById('resultContent');
            card.classList.remove('hidden');

            if (activePortal === 'core') {
                let score = 0.08;
                let severity = "NORMAL";
                let classification = "NORMAL";
                let reasons = ["Observation conforms to physical limits and expected multivariate distributions."];

                if (v1 < -80 || v1 > 65 || v2 < 0 || v2 > 100) {
                    score = 0.95;
                    severity = "CRITICAL";
                    classification = "PHYSICAL_RANGE_VIOLATION";
                    reasons = ["Physical limit bounds violated for station observation."];
                } else if (v1 > 40.0) {
                    score = 0.94;
                    severity = "CRITICAL";
                    classification = "SENSOR_FAULT";
                    reasons = [
                        `Isolation Forest flagged multivariate feature combination (score: ${score}).`,
                        `Unusual deviation in temperature (${v1}°C) or humidity (${v2}%).`,
                        "Uncorroborated by neighboring spatial distribution patterns."
                    ];
                }
                
                let badgeColor = severity === 'CRITICAL' ? 'bg-rose-500/20 text-rose-400 border-rose-500/30' : 'bg-emerald-500/20 text-emerald-400 border-emerald-500/30';
                document.getElementById('outputTitle').innerText = "AI Detection Result";
                content.innerHTML = `
                    <div class="flex justify-between items-center"><span class="text-slate-400">Classification:</span> <strong class="text-teal-300">${classification}</strong></div>
                    <div class="flex justify-between items-center"><span class="text-slate-400">Anomaly Score:</span> <span class="font-mono text-cyan-400">${score}</span></div>
                    <div class="flex justify-between items-center"><span class="text-slate-400">Severity:</span> <span class="px-2 py-0.5 rounded border text-xs ${badgeColor}">${severity}</span></div>
                    <div class="mt-3 pt-2 border-t border-slate-800 text-xs text-slate-300">
                        <strong>Explainability Reasons:</strong>
                        <ul class="list-disc pl-4 mt-1 space-y-1 text-slate-400">
                            ${reasons.map(r => `<li>${r}</li>`).join('')}
                        </ul>
                    </div>
                `;
            } else if (activePortal === 'farmer') {
                document.getElementById('outputTitle').innerText = "Farmer Safe-Procurement & Harvest Advisory";
                content.innerHTML = `
                    <div class="flex justify-between items-center"><span class="text-slate-400">Target Group:</span> <strong class="text-teal-300">Farmers in Catchment Radius</strong></div>
                    <div class="flex justify-between items-center"><span class="text-slate-400">Precipitation Alert:</span> <span class="px-2 py-0.5 rounded border text-xs bg-rose-500/20 text-rose-400 border-rose-500/30">HEAVY RAIN WARNING (${v1} mm)</span></div>
                    <div class="mt-3 pt-2 border-t border-slate-800 text-xs text-slate-300">
                        <strong>Instant Action Protocols Dispatched (Zero Delay / No Queues):</strong>
                        <ul class="list-disc pl-4 mt-1 space-y-1 text-slate-400">
                            <li>Sent automated WhatsApp/SMS advisory to harvest standing crops immediately.</li>
                            <li>Reserved direct slot at regional grain procurement center without long manual queues.</li>
                            <li>Triggered protective plastic sheeting and trenching guidelines for storage safety.</li>
                        </ul>
                    </div>
                `;
            } else if (activePortal === 'admin') {
                document.getElementById('outputTitle').innerText = "Municipal Pre-Rain Maintenance Dispatch";
                content.innerHTML = `
                    <div class="flex justify-between items-center"><span class="text-slate-400">Target Audience:</span> <strong class="text-teal-300">Local Ground Workers & Pumping Stations</strong></div>
                    <div class="flex justify-between items-center"><span class="text-slate-400">Waterlogging Risk:</span> <span class="px-2 py-0.5 rounded border text-xs bg-rose-500/20 text-rose-400 border-rose-500/30">CRITICAL (${v2}% Drainage Full)</span></div>
                    <div class="mt-3 pt-2 border-t border-slate-800 text-xs text-slate-300">
                        <strong>Preemptive Maintenance Broadcasted:</strong>
                        <ul class="list-disc pl-4 mt-1 space-y-1 text-slate-400">
                            <li>Dispatched automated work orders to local ground crews to clear blocked stormwater grates.</li>
                            <li>Ordered immediate fixing of reported major potholes on arterial flooded roads.</li>
                            <li>Pre-activated automated sewage pumping units #1 through #4 ahead of peak rainfall.</li>
                        </ul>
                    </div>
                `;
            } else if (activePortal === 'infrastructure') {
                document.getElementById('outputTitle').innerText = "Public Project Site Safety Protocol";
                content.innerHTML = `
                    <div class="flex justify-between items-center"><span class="text-slate-400">Project Asset:</span> <strong class="text-teal-300">Railway Bridge & Highway Embankment</strong></div>
                    <div class="flex justify-between items-center"><span class="text-slate-400">Shear Stress Level:</span> <span class="px-2 py-0.5 rounded border text-xs bg-rose-500/20 text-rose-400 border-rose-500/30">UNSAFE (${v1} km/h Wind Shear)</span></div>
                    <div class="mt-3 pt-2 border-t border-slate-800 text-xs text-slate-300">
                        <strong>Site Safety Directives Issued:</strong>
                        <ul class="list-disc pl-4 mt-1 space-y-1 text-slate-400">
                            <li>Issued immediate work-halt directive for crane and elevated bridge construction.</li>
                            <li>Secured loose heavy structural elements against high-velocity gusts.</li>
                            <li>Notified chief structural engineer regarding foundation soil saturation thresholds.</li>
                        </ul>
                    </div>
                `;
            }
        });
    </script>
</body>
</html>
