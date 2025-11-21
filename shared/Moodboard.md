
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Shapyfy DNA | Live Moodboard</title>
    
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@600;700;800&family=Outfit:wght@300;400;500;600&display=swap" rel="stylesheet">

    <style>
        /* --- 1. THE PHYSICS (CSS TOKENS) --- */
        :root {
            /* Palette */
            --bg-navy: #2D3142;
            --bg-navy-deep: #1F222E;
            --brand-mint: #A8E6CF;
            --brand-mint-glow: rgba(168, 230, 207, 0.4);
            --brand-lavender: #D4D0E8;
            --alert-coral: #FF9999;
            --text-white: #FFFFFF;
            --text-muted: #CBD5E1;

            /* Materials */
            --glass-bg: rgba(30, 34, 46, 0.6);
            --glass-border: 1px solid rgba(255, 255, 255, 0.08);
            --glass-blur: blur(20px);
            
            /* Shadows */
            --shadow-card: 0px 20px 40px -10px rgba(0, 0, 0, 0.5);
            --shadow-neon: 0px 0px 20px var(--brand-mint-glow);
        }

        /* --- GLOBAL RESET --- */
        * { box-sizing: border-box; margin: 0; padding: 0; }
        
        body {
            background-color: var(--bg-navy-deep);
            color: var(--text-white);
            font-family: 'Outfit', sans-serif;
            padding: 40px;
            /* The Engineering Grid Background */
            background-image: 
                linear-gradient(rgba(255, 255, 255, 0.03) 1px, transparent 1px),
                linear-gradient(90deg, rgba(255, 255, 255, 0.03) 1px, transparent 1px);
            background-size: 40px 40px;
            /* The Aurora Glow */
            background: radial-gradient(circle at 50% -20%, #2A4555 0%, var(--bg-navy-deep) 60%);
            min-height: 100vh;
        }

        /* --- TYPOGRAPHY --- */
        h1, h2, h3 { font-family: 'Montserrat', sans-serif; letter-spacing: -0.02em; }
        
        .dna-header {
            margin-bottom: 60px;
            text-align: center;
        }
        .dna-header h1 { font-size: 3rem; margin-bottom: 10px; }
        .dna-header span { color: var(--brand-mint); text-transform: uppercase; letter-spacing: 2px; font-weight: 600; font-size: 0.8rem; }

        /* --- LAYOUT --- */
        .grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 40px;
            max-width: 1200px;
            margin: 0 auto;
        }

        .section-title {
            grid-column: 1 / -1;
            font-size: 0.9rem;
            text-transform: uppercase;
            letter-spacing: 1.5px;
            color: var(--text-muted);
            border-bottom: 1px solid rgba(255,255,255,0.1);
            padding-bottom: 10px;
            margin-top: 20px;
        }

        /* --- COMPONENT: COLOR SWATCHES --- */
        .color-row { display: flex; gap: 20px; flex-wrap: wrap; }
        .swatch {
            width: 100px;
            height: 100px;
            border-radius: 16px;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            padding: 10px;
            font-size: 0.75rem;
            font-weight: 600;
            box-shadow: 0 10px 20px rgba(0,0,0,0.2);
        }
        .swatch span { background: rgba(0,0,0,0.5); padding: 4px 8px; border-radius: 8px; align-self: flex-start; backdrop-filter: blur(5px); }
        
        .bg-mint { background: var(--brand-mint); color: var(--bg-navy); }
        .bg-navy { background: var(--bg-navy); border: 1px solid rgba(255,255,255,0.1); }
        .bg-lavender { background: var(--brand-lavender); color: var(--bg-navy); }
        .bg-coral { background: var(--alert-coral); color: var(--bg-navy); }

        /* --- COMPONENT: BUTTONS --- */
        .btn {
            padding: 16px 32px;
            border-radius: 12px;
            font-family: 'Montserrat', sans-serif;
            font-weight: 700;
            cursor: pointer;
            border: none;
            transition: all 0.3s ease;
            display: inline-block;
        }

        .btn-primary {
            background: var(--brand-mint);
            color: var(--bg-navy);
            box-shadow: var(--shadow-neon);
            position: relative;
            overflow: hidden;
        }
        /* The Shimmer Effect */
        .btn-primary::after {
            content: '';
            position: absolute;
            top: 0; left: -100%;
            width: 50%; height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.6), transparent);
            transform: skewX(-20deg);
            animation: shimmer 3s infinite;
        }

        .btn-glass {
            background: rgba(255,255,255,0.05);
            color: var(--text-white);
            border: 1px solid rgba(255,255,255,0.1);
        }
        .btn-glass:hover { background: rgba(255,255,255,0.1); border-color: var(--text-white); }

        @keyframes shimmer { 0% { left: -100%; } 20% { left: 200%; } 100% { left: 200%; } }

        /* --- COMPONENT: GLASS CARDS --- */
        .card {
            background: linear-gradient(145deg, rgba(58, 61, 82, 0.4) 0%, rgba(31, 34, 46, 0.6) 100%);
            border: var(--glass-border);
            border-radius: 20px;
            padding: 30px;
            backdrop-filter: var(--glass-blur);
            box-shadow: var(--shadow-card);
            transition: transform 0.3s ease;
        }
        .card:hover { transform: translateY(-5px); border-color: rgba(255,255,255,0.2); }

        .card h3 { margin-bottom: 10px; font-size: 1.2rem; }
        .card p { color: var(--text-muted); line-height: 1.6; font-size: 0.95rem; }

        /* Problem Card Variant */
        .card-problem { border-left: 4px solid var(--alert-coral); }
        .card-problem .icon { color: var(--alert-coral); font-size: 1.5rem; margin-bottom: 15px; }

        /* Intelligence Card Variant */
        .card-ai { border: 1px solid rgba(212, 208, 232, 0.3); background: linear-gradient(145deg, rgba(212, 208, 232, 0.05), rgba(31, 34, 46, 0.6)); }
        .card-ai .tag { color: var(--brand-lavender); font-size: 0.7rem; text-transform: uppercase; letter-spacing: 1px; margin-bottom: 8px; display: block;}

        /* --- COMPONENT: INPUTS (NUMPAD STYLE) --- */
        .input-display {
            background: rgba(0,0,0,0.3);
            border: 1px solid rgba(255,255,255,0.1);
            border-radius: 12px;
            padding: 20px;
            font-family: 'Outfit', sans-serif; /* Monospace vibe */
            font-size: 1.5rem;
            color: var(--text-white);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .cursor { width: 2px; height: 24px; background: var(--brand-mint); animation: blink 1s infinite; }
        
        @keyframes blink { 50% { opacity: 0; } }

    </style>
</head>
<body>

    <div class="dna-header">
        <span>The Shapyfy DNA</span>
        <h1>Visual System 1.0</h1>
        <p style="color: var(--text-muted);">Premium Dark Mode • Glassmorphism • Neon Utility</p>
    </div>

    <div class="grid">
        
        <div class="section-title">1. The Palette (Materials)</div>
        <div class="color-row" style="grid-column: 1 / -1;">
            <div class="swatch bg-mint"><span>Action</span></div>
            <div class="swatch bg-navy"><span>Base</span></div>
            <div class="swatch bg-lavender"><span>AI Brain</span></div>
            <div class="swatch bg-coral"><span>Alert</span></div>
        </div>

        <div class="section-title">2. Typography (Voice)</div>
        <div class="card" style="grid-column: 1 / -1;">
            <h1 style="font-size: 2.5rem; margin-bottom: 10px;">The Speed of a Tracker.</h1>
            <h2 style="color: var(--brand-mint); margin-bottom: 20px;">The Brain of a Coach.</h2>
            <p style="max-width: 600px;">
                Shapyfy is the first offline-first strength tool that adapts to your progress. 
                Get personalized AI recommendations without the loading screens. 
                <br><br>
                <small style="font-family: 'Outfit'; color: var(--text-muted); opacity: 0.6;">
                    Heading: Montserrat Bold (700) | Body: Outfit Regular (400)
                </small>
            </p>
        </div>

        <div class="section-title">3. Interaction (Buttons & Inputs)</div>
        <div style="display: flex; gap: 20px; align-items: center;">
            <button class="btn btn-primary">Join Waitlist</button>
            <button class="btn btn-glass">Learn More</button>
        </div>
        
        <div>
            <div style="margin-bottom: 8px; font-size: 0.8rem; color: var(--text-muted);">ACTIVE INPUT STATE</div>
            <div class="input-display">
                <span>102.5 kg</span>
                <div class="cursor"></div>
            </div>
        </div>

        <div class="section-title">4. UI Components (Cards)</div>
        
        <div class="card card-problem">
            <div class="icon">⚡</div> <h3>The Infinite Load</h3>
            <p>Gym basements are dead zones. Most AI apps leave you staring at a spinning wheel. We are 100% local.</p>
        </div>

        <div class="card card-ai">
            <span class="tag">Smart Suggestion</span>
            <h3>Auto-Regulation</h3>
            <p>Your last session was easy (RPE 6). Increasing load by <strong>2.5kg</strong> is recommended.</p>
        </div>

    </div>

</body>
</html>