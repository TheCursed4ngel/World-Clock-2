<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cyber World Clock</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;500;700;900&family=Rajdhani:wght@300;400;600&display=swap');

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Orbitron', sans-serif;
            background: #0a0a0f;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
            position: relative;
            overflow-x: hidden;
        }

        .cyber-bg {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: 
                linear-gradient(90deg, rgba(0, 255, 255, 0.03) 1px, transparent 1px),
                linear-gradient(0deg, rgba(255, 0, 255, 0.03) 1px, transparent 1px);
            background-size: 40px 40px;
            z-index: 0;
            animation: gridShift 20s linear infinite;
        }

        @keyframes gridShift {
            0% { background-position: 0 0; }
            100% { background-position: 40px 40px; }
        }

        .particle {
            position: fixed;
            width: 2px;
            height: 2px;
            background: rgba(0, 255, 255, 0.3);
            border-radius: 50%;
            pointer-events: none;
            z-index: 0;
            animation: floatParticle 15s infinite linear;
        }

        @keyframes floatParticle {
            from {
                transform: translateY(100vh) translateX(0);
                opacity: 1;
            }
            to {
                transform: translateY(-100vh) translateX(100px);
                opacity: 0;
            }
        }

        .orb {
            position: fixed;
            border-radius: 50%;
            filter: blur(100px);
            z-index: 0;
            animation: orbFloat 20s infinite alternate;
        }

        .orb-1 {
            width: 500px;
            height: 500px;
            background: rgba(0, 255, 255, 0.15);
            top: -200px;
            left: -200px;
        }

        .orb-2 {
            width: 400px;
            height: 400px;
            background: rgba(255, 0, 255, 0.15);
            bottom: -150px;
            right: -150px;
            animation-delay: -5s;
        }

        @keyframes orbFloat {
            from { transform: translate(0, 0) scale(1); }
            to { transform: translate(50px, 50px) scale(1.2); }
        }

        .container {
            position: relative;
            z-index: 1;
            width: 100%;
            max-width: 1200px;
            background: rgba(10, 10, 20, 0.7);
            backdrop-filter: blur(10px);
            border: 2px solid rgba(0, 255, 255, 0.3);
            border-radius: 40px;
            padding: 40px;
            box-shadow: 0 0 50px rgba(0, 255, 255, 0.2);
        }

        .header {
            text-align: center;
            margin-bottom: 40px;
        }

        .glitch-title {
            font-size: clamp(2rem, 6vw, 3.5rem);
            font-weight: 900;
            text-transform: uppercase;
            position: relative;
            text-shadow: 
                0.05em 0 0 rgba(255, 0, 255, 0.75),
                -0.05em -0.05em 0 rgba(0, 255, 255, 0.75);
            animation: glitch 3s infinite;
            letter-spacing: 5px;
        }

        .glitch-title::before,
        .glitch-title::after {
            content: "🌍 WORLD CLOCK 🌍";
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
        }

        .glitch-title::before {
            animation: glitchTop 3s infinite;
            color: #ff00ff;
            text-shadow: 2px 0 #00ffff;
            clip-path: polygon(0 0, 100% 0, 100% 33%, 0 33%);
        }

        .glitch-title::after {
            animation: glitchBottom 3s infinite;
            color: #00ffff;
            text-shadow: -2px 0 #ff00ff;
            clip-path: polygon(0 67%, 100% 67%, 100% 100%, 0 100%);
        }

        @keyframes glitch {
            0%, 100% { transform: skew(0deg); }
            95% { transform: skew(2deg); }
            96% { transform: skew(-2deg); }
        }

        @keyframes glitchTop {
            0%, 100% { transform: translate(0); }
            95% { transform: translate(2px, -2px); }
            96% { transform: translate(-2px, 2px); }
        }

        @keyframes glitchBottom {
            0%, 100% { transform: translate(0); }
            95% { transform: translate(-2px, 2px); }
            96% { transform: translate(2px, -2px); }
        }

        .subtitle {
            font-family: 'Rajdhani', sans-serif;
            color: rgba(255, 255, 255, 0.7);
            margin-top: 10px;
            font-size: 1.2rem;
        }

        .subtitle span {
            color: #00ffff;
            text-shadow: 0 0 10px #00ffff;
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.5; }
        }

        .mode-toggle {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-bottom: 30px;
        }

        .mode-btn {
            padding: 10px 25px;
            background: rgba(0, 0, 0, 0.3);
            border: 2px solid rgba(0, 255, 255, 0.3);
            border-radius: 30px;
            color: white;
            font-family: 'Rajdhani', sans-serif;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
        }

        .mode-btn:hover {
            background: rgba(0, 255, 255, 0.1);
            border-color: #00ffff;
        }

        .mode-btn.active {
            background: linear-gradient(135deg, #00ffff, #ff00ff);
            border-color: transparent;
            box-shadow: 0 0 20px rgba(0, 255, 255, 0.5);
        }

        .clocks-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 30px;
            margin-bottom: 40px;
        }

        .clock-card {
            background: rgba(0, 0, 0, 0.3);
            border: 2px solid transparent;
            border-radius: 30px;
            padding: 25px;
            text-align: center;
            transition: all 0.3s;
            position: relative;
            overflow: hidden;
            animation: cardAppear 0.5s ease-out;
        }

        @keyframes cardAppear {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .clock-card:hover {
            transform: translateY(-5px) scale(1.02);
            border-color: #00ffff;
            box-shadow: 0 0 30px rgba(0, 255, 255, 0.3);
        }

        .clock-card::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: linear-gradient(45deg, transparent, rgba(0, 255, 255, 0.05), transparent);
            transform: rotate(45deg);
            animation: cardShine 3s infinite;
        }

        @keyframes cardShine {
            0% { transform: translateX(-100%) rotate(45deg); }
            100% { transform: translateX(100%) rotate(45deg); }
        }

        .city-icon {
            font-size: 3rem;
            margin-bottom: 15px;
            filter: drop-shadow(0 0 15px currentColor);
        }

        .city-name {
            font-size: 1.8rem;
            font-weight: 700;
            margin-bottom: 5px;
            background: linear-gradient(135deg, #00ffff, #ff00ff);
            background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .country {
            font-family: 'Rajdhani', sans-serif;
            color: rgba(255, 255, 255, 0.5);
            font-size: 0.9rem;
            margin-bottom: 20px;
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        .time {
            font-size: 3rem;
            font-weight: 900;
            color: #00ffff;
            text-shadow: 0 0 20px #00ffff, 0 0 40px #ff00ff;
            line-height: 1.2;
            margin-bottom: 5px;
            font-family: 'Orbitron', monospace;
            animation: timeGlow 2s infinite alternate;
        }

        @keyframes timeGlow {
            from { text-shadow: 0 0 20px #00ffff; }
            to { text-shadow: 0 0 40px #ff00ff, 0 0 60px #00ffff; }
        }

        .date {
            font-family: 'Rajdhani', sans-serif;
            font-size: 1.1rem;
            color: rgba(255, 255, 255, 0.7);
            margin-bottom: 15px;
        }

        .timezone {
            font-family: 'Rajdhani', sans-serif;
            font-size: 0.9rem;
            color: rgba(255, 255, 255, 0.4);
            padding: 5px 10px;
            background: rgba(0, 255, 255, 0.1);
            border-radius: 20px;
            display: inline-block;
        }

        .time-diff {
            position: absolute;
            top: 15px;
            right: 15px;
            font-family: 'Rajdhani', sans-serif;
            font-size: 0.9rem;
            padding: 5px 10px;
            background: rgba(0, 0, 0, 0.5);
            border-radius: 15px;
            color: #ffff00;
            border: 1px solid #ffff00;
        }

        .digital-clock {
            background: rgba(0, 0, 0, 0.5);
            border: 2px solid rgba(0, 255, 255, 0.3);
            border-radius: 60px;
            padding: 30px;
            margin-bottom: 30px;
            text-align: center;
            position: relative;
            overflow: hidden;
        }

        .digital-time {
            font-size: 5rem;
            font-weight: 900;
            color: #00ffff;
            text-shadow: 0 0 30px #00ffff, 0 0 60px #ff00ff;
            font-family: 'Orbitron', monospace;
            line-height: 1;
            letter-spacing: 5px;
            animation: digitalGlow 1s infinite alternate;
        }

        @keyframes digitalGlow {
            from { opacity: 1; }
            to { opacity: 0.9; }
        }

        .digital-date {
            font-family: 'Rajdhani', sans-serif;
            font-size: 1.5rem;
            color: rgba(255, 255, 255, 0.7);
            margin-top: 10px;
        }

        .digital-location {
            font-family: 'Rajdhani', sans-serif;
            color: #ff00ff;
            margin-top: 5px;
            text-transform: uppercase;
            letter-spacing: 3px;
        }

        .controls {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-top: 30px;
            flex-wrap: wrap;
        }

        .control-btn {
            padding: 12px 25px;
            background: rgba(0, 0, 0, 0.3);
            border: 2px solid rgba(0, 255, 255, 0.3);
            border-radius: 30px;
            color: white;
            font-family: 'Rajdhani', sans-serif;
            font-size: 1rem;
            cursor: pointer;
            transition: all 0.3s;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .control-btn:hover {
            background: rgba(0, 255, 255, 0.1);
            border-color: #00ffff;
            transform: translateY(-2px);
        }

        .control-btn i {
            font-size: 1.2rem;
        }

        .footer {
            text-align: center;
            margin-top: 40px;
            padding-top: 20px;
            border-top: 2px solid rgba(0, 255, 255, 0.2);
            font-family: 'Rajdhani', sans-serif;
            color: rgba(255, 255, 255, 0.5);
        }

        .footer span {
            color: #00ffff;
            text-shadow: 0 0 10px #00ffff;
        }

        .update-status {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background: rgba(0, 0, 0, 0.8);
            backdrop-filter: blur(10px);
            padding: 10px 20px;
            border-radius: 30px;
            border: 2px solid #00ffff;
            color: #00ffff;
            font-family: 'Rajdhani', sans-serif;
            font-size: 0.9rem;
            z-index: 1000;
            animation: borderPulse 2s infinite;
        }

        @keyframes borderPulse {
            0%, 100% { border-color: #00ffff; }
            50% { border-color: #ff00ff; }
        }

        @media (max-width: 768px) {
            .container {
                padding: 20px;
            }

            .digital-time {
                font-size: 3rem;
            }

            .time {
                font-size: 2.2rem;
            }

            .clocks-grid {
                gap: 15px;
            }

            .controls {
                flex-direction: column;
                align-items: center;
            }

            .control-btn {
                width: 100%;
                justify-content: center;
            }
        }
    </style>
</head>
<body>
    <div id="particles"></div>
    <div class="cyber-bg"></div>
    <div class="orb orb-1"></div>
    <div class="orb orb-2"></div>

    <div class="container">
        <div class="header">
            <h1 class="glitch-title">🌍 WORLD CLOCK 🌍</h1>
            <div class="subtitle">
                <span>⚡</span> REAL TIME • MULTIPLE TIMEZONES <span>⚡</span>
            </div>
        </div>

        <div class="mode-toggle">
            <button class="mode-btn active" onclick="setTimeFormat('24h', this)">24H</button>
            <button class="mode-btn" onclick="setTimeFormat('12h', this)">12H</button>
        </div>

        <div class="digital-clock" id="digitalClock">
            <div class="digital-time" id="digitalTime">--:--:--</div>
            <div class="digital-date" id="digitalDate">---</div>
            <div class="digital-location" id="digitalLocation">LOCAL TIME</div>
        </div>

        <div class="clocks-grid" id="clocksGrid"></div>

        <div class="controls">
            <button class="control-btn" onclick="addCity()">
                <i>➕</i> ADD CITY
            </button>
            <button class="control-btn" onclick="resetCities()">
                <i>🔄</i> RESET
            </button>
            <button class="control-btn" onclick="refreshAll()">
                <i>⚡</i> SYNC
            </button>
        </div>

        <div class="footer">
            <span>🌐</span> UPDATED IN REAL TIME • EXACT TIME <span>🌐</span>
        </div>
    </div>

    <div class="update-status" id="updateStatus">
        ⚡ SYNCHRONIZED
    </div>

    <script>
        const defaultCities = [
            { 
                name: 'Paris', 
                country: 'France', 
                timezone: 'Europe/Paris', 
                icon: '🇫🇷',
                flag: '🇫🇷'
            },
            { 
                name: 'New York', 
                country: 'United States', 
                timezone: 'America/New_York', 
                icon: '🗽',
                flag: '🇺🇸'
            },
            { 
                name: 'Tokyo', 
                country: 'Japan', 
                timezone: 'Asia/Tokyo', 
                icon: '🗼',
                flag: '🇯🇵'
            },
            { 
                name: 'London', 
                country: 'United Kingdom', 
                timezone: 'Europe/London', 
                icon: '🇬🇧',
                flag: '🇬🇧'
            },
            { 
                name: 'Sydney', 
                country: 'Australia', 
                timezone: 'Australia/Sydney', 
                icon: '🦘',
                flag: '🇦🇺'
            },
            { 
                name: 'Moscow', 
                country: 'Russia', 
                timezone: 'Europe/Moscow', 
                icon: '🇷🇺',
                flag: '🇷🇺'
            },
            { 
                name: 'Dubai', 
                country: 'UAE', 
                timezone: 'Asia/Dubai', 
                icon: '🇦🇪',
                flag: '🇦🇪'
            },
            { 
                name: 'Singapore', 
                country: 'Singapore', 
                timezone: 'Asia/Singapore', 
                icon: '🇸🇬',
                flag: '🇸🇬'
            }
        ];

        let cities = [];
        let timeFormat = '24h';
        let updateInterval;

        function init() {
            loadCities();
            createParticles();
            startClock();
            updateAllClocks();
        }

        function loadCities() {
            const saved = localStorage.getItem('worldCities');
            if (saved) {
                cities = JSON.parse(saved);
            } else {
                cities = [...defaultCities];
            }
            renderClocks();
        }

        function saveCities() {
            localStorage.setItem('worldCities', JSON.stringify(cities));
        }

        function createParticles() {
            const particlesContainer = document.getElementById('particles');
            for (let i = 0; i < 50; i++) {
                const particle = document.createElement('div');
                particle.className = 'particle';
                particle.style.left = Math.random() * 100 + '%';
                particle.style.animationDelay = Math.random() * 10 + 's';
                particle.style.animationDuration = (10 + Math.random() * 20) + 's';
                particlesContainer.appendChild(particle);
            }
        }

        function startClock() {
            if (updateInterval) clearInterval(updateInterval);
            updateInterval = setInterval(updateAllClocks, 1000);
        }

        function updateAllClocks() {
            updateDigitalClock();
            updateCityClocks();
        }

        function updateDigitalClock() {
            const now = new Date();
            const hours = now.getHours();
            const minutes = now.getMinutes();
            const seconds = now.getSeconds();
            
            let timeString;
            if (timeFormat === '24h') {
                timeString = `${formatNumber(hours)}:${formatNumber(minutes)}:${formatNumber(seconds)}`;
            } else {
                const ampm = hours >= 12 ? 'PM' : 'AM';
                const hour12 = hours % 12 || 12;
                timeString = `${formatNumber(hour12)}:${formatNumber(minutes)}:${formatNumber(seconds)} ${ampm}`;
            }

            document.getElementById('digitalTime').textContent = timeString;
            document.getElementById('digitalDate').textContent = now.toLocaleDateString('en-US', { 
                weekday: 'long', 
                year: 'numeric', 
                month: 'long', 
                day: 'numeric' 
            });
        }

        function updateCityClocks() {
            const now = new Date();
            
            cities.forEach((city, index) => {
                const clockElement = document.getElementById(`clock-${index}`);
                if (clockElement) {
                    try {
                        const cityTime = new Date().toLocaleString('en-US', { 
                            timeZone: city.timezone,
                            hour: '2-digit',
                            minute: '2-digit',
                            second: '2-digit',
                            hour12: timeFormat === '12h'
                        });

                        const cityDate = new Date().toLocaleString('en-US', { 
                            timeZone: city.timezone,
                            weekday: 'short',
                            day: '2-digit',
                            month: 'short'
                        });

                        const timeElement = clockElement.querySelector('.time');
                        const dateElement = clockElement.querySelector('.date');
                        
                        if (timeElement) timeElement.textContent = cityTime;
                        if (dateElement) dateElement.textContent = cityDate;

                        const localHours = now.getHours();
                        const cityHours = parseInt(new Date().toLocaleString('en-US', { 
                            timeZone: city.timezone,
                            hour: '2-digit',
                            hour12: false 
                        }));
                        
                        let diff = cityHours - localHours;
                        if (diff > 12) diff -= 24;
                        if (diff < -12) diff += 24;
                        
                        const diffElement = clockElement.querySelector('.time-diff');
                        if (diffElement) {
                            const diffText = diff > 0 ? `+${diff}h` : `${diff}h`;
                            diffElement.textContent = diffText;
                        }
                    } catch (e) {
                        console.log('Error for', city.name);
                    }
                }
            });

            document.getElementById('updateStatus').innerHTML = '⚡ SYNCHRONIZED';
            setTimeout(() => {
                document.getElementById('updateStatus').innerHTML = '🌐 LIVE';
            }, 1000);
        }

        function renderClocks() {
            const grid = document.getElementById('clocksGrid');
            grid.innerHTML = '';

            cities.forEach((city, index) => {
                try {
                    const cityTime = new Date().toLocaleString('en-US', { 
                        timeZone: city.timezone,
                        hour: '2-digit',
                        minute: '2-digit',
                        second: '2-digit',
                        hour12: timeFormat === '12h'
                    });

                    const cityDate = new Date().toLocaleString('en-US', { 
                        timeZone: city.timezone,
                        weekday: 'short',
                        day: '2-digit',
                        month: 'short',
                        year: 'numeric'
                    });
                    const now = new Date();
                    const localHours = now.getHours();
                    const cityHours = parseInt(new Date().toLocaleString('en-US', { 
                        timeZone: city.timezone,
                        hour: '2-digit',
                        hour12: false 
                    }));
                    
                    let diff = cityHours - localHours;
                    if (diff > 12) diff -= 24;
                    if (diff < -12) diff += 24;

                    const card = document.createElement('div');
                    card.className = 'clock-card';
                    card.id = `clock-${index}`;
                    card.innerHTML = `
                        <div class="city-icon">${city.icon || city.flag || '🌍'}</div>
                        <div class="city-name">${city.name}</div>
                        <div class="country">${city.country}</div>
                        <div class="time">${cityTime}</div>
                        <div class="date">${cityDate}</div>
                        <div class="timezone">${city.timezone}</div>
                        <div class="time-diff">${diff > 0 ? `+${diff}h` : `${diff}h`}</div>
                        <button class="control-btn" style="margin-top: 15px; width: 100%;" onclick="removeCity(${index})">
                            <i>🗑️</i> REMOVE
                        </button>
                    `;

                    grid.appendChild(card);
                } catch (e) {
                    console.log('Error for', city.name);
                }
            });
        }

        function formatNumber(num) {
            return num.toString().padStart(2, '0');
        }

        function setTimeFormat(format, btn) {
            timeFormat = format;
            document.querySelectorAll('.mode-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            btn.classList.add('active');
            updateAllClocks();
            renderClocks();
        }

        function addCity() {
            const cityName = prompt('City name:', 'Dubai');
            if (!cityName) return;

            const timezone = prompt('Timezone (e.g., Asia/Dubai):', 'Asia/Dubai');
            if (!timezone) return;

            const country = prompt('Country:', 'UAE');
            const flag = prompt('Flag (emoji):', '🇦🇪');

            const newCity = {
                name: cityName,
                country: country,
                timezone: timezone,
                icon: flag,
                flag: flag
            };

            cities.push(newCity);
            saveCities();
            renderClocks();
            showStatus('CITY ADDED');
        }

        function removeCity(index) {
            if (confirm(`Remove ${cities[index].name}?`)) {
                cities.splice(index, 1);
                saveCities();
                renderClocks();
                showStatus('CITY REMOVED');
            }
        }

        function resetCities() {
            if (confirm('Reset to default cities?')) {
                cities = [...defaultCities];
                saveCities();
                renderClocks();
                showStatus('RESET');
            }
        }

        function refreshAll() {
            updateAllClocks();
            showStatus('SYNCING...');
        }

        function showStatus(message) {
            const status = document.getElementById('updateStatus');
            status.innerHTML = `⚡ ${message}`;
            status.style.borderColor = '#ffff00';
            status.style.color = '#ffff00';
            
            setTimeout(() => {
                status.innerHTML = '🌐 LIVE';
                status.style.borderColor = '#00ffff';
                status.style.color = '#00ffff';
            }, 2000);
        }

        window.addEventListener('load', init);

        window.addEventListener('beforeunload', () => {
            if (updateInterval) clearInterval(updateInterval);
        });
    </script>
</body>
</html>
