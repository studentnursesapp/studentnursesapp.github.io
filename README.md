<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>The 30-Day Judgment: Ultimate Dark Mode</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;900&display=swap" rel="stylesheet">
    <style>
        /* Base Dark Theme Foundation */
        html, body {
            height: 100%;
            margin: 0;
            padding: 0;
            overscroll-behavior-y: contain;
        }
        body {
            background: linear-gradient(135deg, #0f172a 0%, #1e293b 50%, #334155 100%); /* Gradient background */
            font-family: 'Inter', 'Segoe UI', sans-serif;
            display: flex;
            flex-direction: column;
            padding: 1rem;
            color: #f1f5f9; /* Primary text */
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1); /* Smooth transitions global */
        }

        /* Keyframes for Shimmer Effect (animating background-position) */
        @keyframes backgroundShine {
            0% { background-position: -200% 0; }
            100% { background-position: 200% 0; }
        }

        /* Glassmorphism Effects & Main Container */
        #app {
            background: rgba(30, 41, 59, 0.8); /* Main container background */
            backdrop-filter: blur(20px); /* Main container blur */
            -webkit-backdrop-filter: blur(20px); /* Safari support */
            border-radius: 16px; /* rounded-2xl */
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.4); /* Enhanced shadow */
            max-width: 1100px;
            margin: 0 auto;
            height: 100%;
            width: 100%;
            display: flex;
            flex-direction: column;
            position: relative;
            border: 1px solid rgba(255, 255, 255, 0.1); /* Subtle border */
        }
        #main-content-wrapper {
            flex-grow: 1;
            overflow-y: auto;
            padding-right: 8px;
        }

        /* Cards & Section Containers (Glassmorphism) */
        .glass-card {
            background: rgba(51, 65, 85, 0.6); /* Card background */
            backdrop-filter: blur(10px); /* Card blur */
            -webkit-backdrop-filter: blur(10px);
            border-radius: 12px; /* rounded-xl */
            border: 1px solid rgba(255, 255, 255, 0.08); /* Subtle border */
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2); /* Layered shadow */
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .task-card, .exam-card {
            position: relative;
            overflow: hidden; /* Hide shimmer background outside */
        }
        .task-card:hover, .exam-card:hover {
            transform: translateX(4px); /* Card hover animation */
            box-shadow: 0 6px 15px rgba(0, 0, 0, 0.3), 0 0 20px rgba(59, 130, 246, 0.3); /* Enhanced shadow with glow */
        }

        /* Text Colors */
        .text-primary { color: #f1f5f9; } /* slate-100 */
        .text-secondary { color: #cbd5e1; } /* slate-300 */
        .text-accent { color: #94a3b8; } /* slate-400 */

        /* Headers with Gradient Text (Corrected to be more visible) */
        .gradient-text {
            background-image: linear-gradient(to right, #a7d9ff, #e0b0ff, #ffdab3); /* Brighter gradient: Light blue, light purple, light orange */
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }

        /* Buttons & Interactive Elements - Base Styles for Shimmer */
        .tab, .day-selector-item, .quick-nav-btn {
            background-size: 200% 100%; /* Make background larger for shimmer */
            background-repeat: no-repeat;
            position: relative;
            overflow: hidden;
            z-index: 1; /* Ensure content is above shimmer */
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .quick-nav-btn.orange {
            background-image: linear-gradient(to right, #fb923c, #f97316); /* Base gradient for orange buttons */
        }

        /* Shimmer Effect Overlay (via pseudo-element animating background-position) */
        .tab::before, .day-selector-item::before, .quick-nav-btn::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0; 
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.3), transparent); /* Shimmer gradient */
            background-size: 200% 100%; /* Make shimmer wider than element */
            background-position: -100% 0; /* Start shimmer off-screen to the left */
            animation: backgroundShine 1.5s infinite linear paused; /* Animation paused by default */
            z-index: 2; /* Over button's base content, but content itself is not transparent */
        }

        .tab:hover::before, .day-selector-item:hover::before, .quick-nav-btn:hover::before {
            animation-play-state: running; /* Run animation on hover */
        }

        /* Tab Specific Styles */
        .tab {
            color: #f1f5f9;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
            border-radius: 12px; /* rounded-xl */
            background-image: linear-gradient(to right, #1d4ed8, #1e40af); /* Darker blue gradient */
            border: 1px solid transparent;
        }
        .tab.active {
            background-image: linear-gradient(to right, #2563eb, #1d4ed8); /* blue-600 to blue-700 */
            box-shadow: 0 4px 8px rgba(0,0,0,0.4), 0 0 15px rgba(59, 130, 246, 0.5); /* Enhanced shadow with glow */
            transform: translateY(-2px); /* Lift on active */
            border: 1px solid rgba(255, 255, 255, 0.2); /* Subtle white border on active */
        }
        .tab:hover {
            transform: translateY(-2px); /* Lift on hover */
            box-shadow: 0 4px 8px rgba(0,0,0,0.4), 0 0 10px rgba(59, 130, 246, 0.4); /* Enhanced shadow with subtle glow */
            color: #ffffff; /* Brighter text on hover */
        }

        /* Day Selector Buttons */
        .day-selector-item {
            background-color: transparent; /* Transparent background */
            color: #f1f5f9; /* Primary text */
            border: none; /* No border for a clean, floating look */
            box-shadow: none; /* Remove default shadow */
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            position: relative; /* For shimmer */
            overflow: hidden; /* For shimmer */
            border-radius: 12px; /* rounded-xl */
        }
        .day-selector-item.active {
            background-image: linear-gradient(to bottom right, #3b82f6, #2563eb); /* Primary blue gradient */
            color: white;
            font-weight: 700;
            transform: scale(1.05); /* Scale on active */
            box-shadow: 0 4px 10px rgba(0,0,0,0.4), 0 0 20px rgba(59, 130, 246, 0.5); /* Stronger glow on active */
            border-color: transparent; /* No border on active */
        }
        .day-selector-item:hover {
            transform: scale(1.05); /* Scale on hover */
            background-color: rgba(59, 130, 246, 0.1); /* Subtle blue tint on hover */
            border: 1px solid rgba(59, 130, 246, 0.2); /* Subtle border on hover */
            box-shadow: 0 2px 8px rgba(0,0,0,0.3);
        }

        /* Quick Navigation Buttons (Timetable) */
        .quick-nav-btn {
            color: #f1f5f9;
            font-weight: 600;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
            border-radius: 9999px; /* rounded-full */
            background-image: linear-gradient(to right, #3b82f6, #2563eb);
        }
        .quick-nav-btn:hover {
            transform: translateY(-2px); /* Lift on hover */
            box-shadow: 0 4px 8px rgba(0,0,0,0.4), 0 0 10px rgba(59, 130, 246, 0.4); /* Enhanced shadow with glow */
        }
        .quick-nav-btn.orange {
            background-image: linear-gradient(to right, #fb923c, #f97316); /* Base gradient for orange buttons */
        }
        .quick-nav-btn.orange:hover {
             box-shadow: 0 4px 8px rgba(0,0,0,0.4), 0 0 10px rgba(251, 146, 60, 0.4); /* Orange glow */
        }

        /* Countdown Numbers - Specific light blue highlight */
        #countdown-text {
            background-image: linear-gradient(to right, #e0f2fe, #bbdefb, #90caf9); /* Very light blue gradient */
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            font-weight: 900;
            letter-spacing: -0.05em;
            text-shadow: 
                0 0 1px rgba(255,255,255,0.8), /* Light overall glow/outline */
                0 0 5px rgba(144, 202, 249, 0.4); /* Subtle blue glow */
        }

        /* Exam Timetable - Visual Hierarchy */
        .exam-card {
            background: rgba(51, 65, 85, 0.6); /* Card background */
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.08);
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .exam-card strong {
            color: #f1f5f9; /* Primary text for bolded dates */
        }
        .exam-card span.text-accent {
            color: #94a3b8; /* Accent text for time, as per prompt */
        }
        .exam-card.border-blue-500 { border-left-color: #3b82f6; } /* Primary Blue for theory */
        .exam-card.border-orange-500 { border-left-color: #fb923c; } /* Secondary Orange for practical */
        
        /* Charts - Adjusted for Dark Mode */
        .chart-container {
            position: relative;
            margin: auto;
            height: 280px;
            width: 100%;
            max-width: 350px;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 320px;
            }
        }
        /* Chart specific styling */
        .chartjs-render-monitor canvas {
            background-color: rgba(51, 65, 85, 0.3) !important; /* Match card background opacity */
            border-radius: 12px;
        }
        .chartjs-render-monitor .chartjs-grid-line {
            stroke: rgba(255, 255, 255, 0.1) !important; /* Lighter grid lines */
        }
        .chartjs-render-monitor .chartjs-tooltip {
            background-color: rgba(30, 41, 59, 0.9) !important;
            color: #f1f5f9 !important;
            border: 1px solid rgba(255, 255, 255, 0.2) !important;
            backdrop-filter: blur(5px) !important;
        }
        .chartjs-render-monitor .chartjs-tooltip-title,
        .chartjs-render-monitor .chartjs-tooltip-label {
            color: #f1f5f9 !important;
        }
        .chartjs-legend-labels li span {
            background-color: inherit !important; /* Fix for legend colors */
            border-radius: 4px;
        }
        .chartjs-legend-labels li {
            color: #cbd5e1 !important; /* Secondary text color for legend labels */
        }
        
        /* Sticky header for daily tasks */
        #plan-details .sticky {
            background: rgba(30, 41, 59, 0.8); /* Match main container background */
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border-bottom: 1px solid rgba(255, 255, 255, 0.1); /* Subtle separator */
            z-index: 10;
        }

        /* Back to Top Button */
        #backToTopBtn {
            position: fixed;
            bottom: 1.5rem;
            right: 1.5rem;
            display: none;
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            opacity: 0;
            transform: translateY(10px);
            z-index: 50;
            background: linear-gradient(to right, #ef4444, #dc2626); /* Red gradient */
            color: white;
            border-radius: 50%;
            width: 50px;
            height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 4px 10px rgba(0,0,0,0.4), 0 0 15px rgba(239, 68, 68, 0.5); /* Red glow */
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        #backToTopBtn.show {
            display: flex;
            opacity: 1;
            transform: translateY(0);
        }
        #backToTopBtn:hover {
            transform: translateY(-5px) scale(1.1); /* More pronounced hover */
            box-shadow: 0 6px 12px rgba(0,0,0,0.5), 0 0 20px rgba(239, 68, 68, 0.7);
        }
        #backToTopBtn svg {
            width: 20px; /* Adjusted icon size to w-5 h-5 */
            height: 20px;
        }

        /* Responsive Padding */
        .p-4 { padding: 1rem; }
        @media (min-width: 640px) { .sm\:p-6 { padding: 1.5rem; } }
        @media (min-width: 768px) { .md\:p-8 { padding: 2rem; } }

        /* Loading Animation */
        .stagger-load-item {
            opacity: 0; /* Starts hidden */
            transform: translateY(20px); /* Starts below */
            transition: opacity 0.5s ease-out, transform 0.5s ease-out; /* Smooth transition */
        }
        .stagger-load-item.loaded {
            opacity: 1; /* Fades in */
            transform: translateY(0); /* Slides up */
        }

    </style>
</head>
<body>

    <div id="app" class="p-4 sm:p-6 md:p-8">
        <header class="text-center mb-4 flex-shrink-0">
            <h1 class="text-3xl sm:text-4xl font-black tracking-tight gradient-text">The 30-Day Path from Rock Bottom to Competent
</h1>
            <p class="mt-2 text-base text-secondary">Interactive Dashboard. Study Well . God Bless You All</p>
        </header>

        <div class="mb-6 p-1 rounded-2xl flex-shrink-0 flex justify-center glass-card">
            <nav id="top-tab-nav" class="flex space-x-1 p-1 rounded-xl" aria-label="Tabs">
                <button data-view="view-mission-schedule" class="tab py-2 px-4 sm:px-6 font-semibold text-sm">Mission Schedule</button>
                <button data-view="view-progress" class="tab py-2 px-4 sm:px-6 font-semibold text-sm">Progress Tracker</button>
                <button data-view="view-timetable" class="tab py-2 px-4 sm:px-6 font-semibold text-sm">Exam Timetable</button>
            </nav>
        </div>

        <div id="main-content-wrapper">
            <main id="main-content">
                <div id="view-mission-schedule">
                    <div class="mb-8 p-4 rounded-xl flex items-center gap-4 glass-card">
                        <div class="w-20 h-20 flex-shrink-0">
                            <canvas id="countdownChart"></canvas>
                        </div>
                        <div class="flex-grow">
                            <h2 class="text-base font-bold text-primary">Time Until First Exam</h2>
                            <div id="countdown-text" class="text-2xl md:text-3xl font-black tracking-tighter">
                                <span id="days">00</span>d : 
                                <span id="hours">00</span>h : 
                                <span id="minutes">00</span>m : 
                                <span id="seconds">00</span>s
                            </div>
                        </div>
                    </div>
                    <div class="flex flex-col lg:flex-row gap-8">
                        <aside class="w-full lg:w-1/3 xl:w-1/4 lg:sticky top-4 self-start glass-card">
                            <h2 class="text-xl font-bold mb-4 gradient-text">Mission Schedule</h2>
                            <div id="day-selector" class="space-y-4 p-4 rounded-xl shadow-sm glass-card"></div>
                        </aside>
                        <section id="plan-details" class="w-full lg:w-2/3 xl:w-3/4"></section>
                    </div>
                </div>
                <div id="view-progress" class="hidden">
                        <div class="text-center mb-8">
                            <h2 class="text-2xl font-bold gradient-text">Your Progress Report</h2>
                            <p class="mt-1 text-secondary">This is a real-time reflection of your work. Make the numbers go up.</p>
                        </div>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-8 items-center">
                        <div class="p-6 rounded-xl shadow-sm glass-card">
                            <h3 class="font-bold text-lg text-center mb-4 text-primary">Overall Completion</h3>
                            <div class="chart-container">
                                <canvas id="overallProgressChart"></canvas>
                            </div>
                        </div>
                        <div class="p-6 rounded-xl shadow-sm glass-card">
                            <h3 class="font-bold text-lg text-center mb-4 text-primary">Weekly Task Completion</h3>
                                <div class="chart-container" style="max-width: 600px;">
                                    <canvas id="weeklyProgressChart"></canvas>
                                </div>
                        </div>
                    </div>
                </div>
                <div id="view-timetable" class="hidden">
                    <div class="p-4 sm:p-6 rounded-xl shadow-sm glass-card">
                        <h2 class="text-2xl font-bold gradient-text mb-4">Final Examination Schedule</h2>
                        <div class="mb-6 p-4 rounded-xl glass-card">
                            <h3 class="font-semibold text-primary mb-3">Quick Navigation</h3>
                            <div class="flex flex-wrap gap-2">
                                <button onclick="scrollToExam('date-18', 'theory')" class="quick-nav-btn py-1 px-3 rounded-full text-xs font-semibold">Aug 18 (Theory)</button>
                                <button onclick="scrollToExam('date-20', 'theory')" class="quick-nav-btn py-1 px-3 rounded-full text-xs font-semibold">Aug 20 (Theory)</button>
                                <button onclick="scrollToExam('date-22', 'theory')" class="quick-nav-btn py-1 px-3 rounded-full text-xs font-semibold">Aug 22 (Theory)</button>
                                <button onclick="scrollToExam('date-23', 'theory')" class="quick-nav-btn py-1 px-3 rounded-full text-xs font-semibold">Aug 23 (Theory)</button>
                                <button onclick="scrollToExam('date-25', 'practical')" class="quick-nav-btn orange py-1 px-3 rounded-full text-xs font-semibold">Practicals</button>
                            </div>
                        </div>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                            <div class="theory">
                                <h2 class="font-semibold text-lg text-primary mb-2">Theory Examinations</h2>
                                <ul class="space-y-4 text-sm sm:text-base">
                                    <li id="date-18" class="exam-card border-l-4 border-blue-500 p-4 rounded-xl">
                                        <strong class="text-primary">18.08.2025 (Monday)</strong><br>
                                        PHAR 205T – Pharmacology (I & II)<br>
                                        PATH 210T – Pathology (I & II) (Including Genetics)<br>
                                        <span class="text-accent">Time:</span> 10:00 A.M. – 1:00 P.M.
                                    </li>
                                    <li id="date-20" class="exam-card border-l-4 border-blue-500 p-4 rounded-xl">
                                        <strong class="text-primary">20.08.2025 (Wednesday)</strong><br>
                                        N-AHN 225T – Adult Health Nursing II with Integrated Pathophysiology, Geriatric Nursing & Palliative Care<br>
                                        <span class="text-accent">Time:</span> 10:00 A.M. – 1:00 P.M.
                                    </li>
                                    <li id="date-22" class="exam-card border-l-4 border-blue-500 p-4 rounded-xl">
                                        <strong class="text-primary">22.08.2025 (Friday)</strong><br>
                                        PROF 230 – Professionalism, Professional Values & Ethics (Including Bioethics)<br>
                                        <span class="text-accent">Time:</span> 10:00 A.M. – 1:00 P.M.
                                    </li>
                                    <li id="date-23" class="exam-card border-l-4 border-blue-500 p-4 rounded-xl">
                                        <strong class="text-primary">23.08.2025 (Saturday)</strong><br>
                                        ELEC 1 – Human Values (Elective)<br>
                                        <span class="text-accent">Time:</span> 10:00 A.M. – 1:00 P.M.
                                    </li>
                                </ul>
                            </div>
                            <div class="practical">
                                <h2 class="font-semibold text-lg text-primary mb-2">Practical Examinations</h2>
                                <ul class="space-y-4 text-sm sm:text-base">
                                    <li id="date-25" class="exam-card border-l-4 border-orange-500 p-4 rounded-xl">
                                        <strong class="text-primary">25.08.2025 (Monday)</strong><br>
                                        N-AHN 225P – Adult Health Nursing II Practical
                                    </li>
                                    <li id="date-26" class="exam-card border-l-4 border-orange-500 p-4 rounded-xl">
                                        <strong class="text-primary">26.08.2025 (Tuesday)</strong><br>
                                        N-AHN 225P – Adult Health Nursing II Practical
                                    </li>
                                    <li id="date-28" class="exam-card border-l-4 border-orange-500 p-4 rounded-xl">
                                        <strong class="text-primary">28.08.2025 (Thursday)</strong><br>
                                        N-AHN 225P – Adult Health Nursing II Practical
                                    </li>
                                    <li id="date-29" class="exam-card border-l-4 border-orange-500 p-4 rounded-xl">
                                        <strong class="text-primary">29.08.2025 (Friday)</strong><br>
                                        N-AHN 225P – Adult Health Nursing II Practical
                                    </li>
                                </ul>
                            </div>
                        </div>
                    </div>
                </div>
            </main>
        </div>
        
        <button id="backToTopBtn">
            <svg xmlns="http://www.w3.org/2000/svg" class="w-5 h-5" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 15l7-7 7 7" /></svg>
        </button>
    </div>

<script>
const planData = [
    { day: 1, week: 1, title: "THE PURGE", tasks: [
        { subject: "Pharmacology I (3rd Sem)", details: "Unit 1: Routes of administration (Essay, Oct 2022); Advantages/Disadvantages of IV & Oral routes (Notes, Aug 2014; SAQ, Feb 2014); Parenteral routes (SA, Aug 2012)." },
        { subject: "Pathology I (3rd Sem)", details: "Unit I: Importance of pathology; Definition of terms; Etiology & pathogenesis of reversible/irreversible cell injury." },
        { subject: "Med-Surg", details: "Unit 1 (ENT): Define hearing loss; Classify hearing loss & its etiological factors; Clinical manifestations & management of deafness (Essay, Sep 2015)." },
        { subject: "Professionalism", details: "Unit I: Define 'Profession' & its criteria (VSN, Sess. I; MCQ, Sess. I)." },
        { subject: "Genetics", details: "Unit I: Practical application of genetics in nursing; Impact on families; Cellular division: mitosis & meiosis." },
        { subject: "Pharmacology I (3rd Sem)", details: "Unit 1: Factors affecting drug absorption (Essay, Apr 2013); Define first-pass metabolism (VSN, Sess. I)." },
        { subject: "Pathology I (3rd Sem)", details: "Unit 1: Necrosis (SN, Sep 2017; SN, Feb 2009); Types of necrosis with organ examples (Essay, Oct 2022; SN, Feb 2013)." }
    ]},
    { day: 2, week: 1, title: "THE PURGE", tasks: [
        { subject: "Pharmacology I (3rd Sem)", details: "Unit 1: Principles of drug admin (Notes, Aug 2024; Essay, Sep 2015); Pharmacodynamics, Drug Antagonism, Synergism, Tolerance, Receptors." },
        { subject: "Pathology I (3rd Sem)", details: "Unit I: Gangrene (SN, Feb 2010; SN, Sep 2014; SN, Mar 2014); Differences between dry & wet gangrene (Essay, Aug 2024); Dry gangrene (SA, Aug 2012)." },
        { subject: "Med-Surg", details: "Unit 1 (ENT): Management of CSOM: causes, risk factors, pathology, complications, medical, surgical, nursing management (Essay, Aug 2024)." },
        { subject: "Professionalism", details: "Unit I: Characteristics of the Nursing profession (Essay, Sess. I); Differentiate personal & professional identity (SN, Sess. I)." },
        { subject: "Genetics", details: "Unit I: Characteristics and structure of genes; Chromosomes & sex determination." },
        { subject: "Pharmacology I (3rd Sem)", details: "Unit 2: Classify Antiseptics (Essay, Oct 2022); Examples of disinfectants (VSN, Sess. II)." },
        { subject: "Pathology I (3rd Sem)", details: "Unit I: Cellular adaptations: Atrophy, Hypertrophy, Hyperplasia, Metaplasia (Define w/ examples, SA, Aug 2024), Dysplasia." }
    ]},
    { day: 3, week: 1, title: "THE PURGE", tasks: [
        { subject: "Pharmacology I (3rd Sem)", details: "Unit 3: Emetics & Antiemetics; Name 4 anti-emetic drugs (VSN, Apr 2025); Prokinetics (Metoclopramide); Anti-motion sickness drugs (Scopolamine/Hyoscine)." },
        { subject: "Pathology I (3rd Sem)", details: "Unit I: Apoptosis: definition & mechanism (SN, Sep 2018)." },
        { subject: "Med-Surg", details: "Unit 1 (ENT): SN on Otitis media (Sep 2017; May 2024); Infection of the middle ear (MCQ, Mar 2016)." },
        { subject: "Professionalism", details: "Unit I: Define Professionalism & its characteristics (VSN, Aug 2024; SN, Sess. I); Challenges of Professionalism (VSN, Sess. I)." },
        { subject: "Genetics", details: "Unit I: Chromosomal aberrations; Essay/SN on Down Syndrome (Trisomy 21) (SN, Apr 2025)." },
        { subject: "Pharmacology I (3rd Sem)", details: "Unit 3: Classify drugs for Peptic ulcer (Essay, Oct 2024); Antacids (SN, Aug 2012; SN, Sep 2017; SN, Feb 2009)." },
        { subject: "Pathology I (3rd Sem)", details: "Unit I: Acute & Chronic inflammation (Vascular/Cellular events, systemic effects); Granulomatous inflammation." }
    ]},
    { day: 4, week: 1, title: "THE PURGE", tasks: [
        { subject: "Pharmacology I (3rd Sem)", details: "Unit 3: MOA, uses, adverse effects of PPIs (SA, Sess. I; Essay, Mar 2014); H2 blockers (Cimetidine)." },
        { subject: "Pathology I (3rd Sem)", details: "Unit I: Phagocytosis (SN, Sep 2017; SN, Aug 2014); Systemic effects of inflammation." },
        { subject: "Med-Surg", details: "Unit 1 (ENT): Meniere's disease (SN, Oct 2024; SN, Oct 2010; MCQ, May 2024); Labyrinthitis (SA, Aug 2024); Vertigo (VSN, May 2024; MCQ, Apr 2025)." },
        { subject: "Professionalism", details: "Unit I: Communication & relationship w/ team, patients, society (SN, Sess. II); RESPECT strategies (VSN, Aug 2024)." },
        { subject: "Genetics", details: "Unit I: MCQ on Turner syndrome (Apr 2025); MCQ on primary oocyte constitution (Sep 2017)." },
        { subject: "Pharmacology I (3rd Sem)", details: "Unit 3: Anti-diarrhoeals; Fluid and electrolyte therapy (SN, Mar 2014); Name 4 anti-diarrhoeal drugs (SA, Aug 2014)." },
        { subject: "Pathology I (3rd Sem)", details: "Unit I: Wound healing: types & steps (Essay, Sep 2018; Essay, Feb 2013); Healing by primary intention (SN, Feb 2011)." }
    ]},
    { day: 5, week: 1, title: "THE PURGE", tasks: [
        { subject: "Pharmacology I (3rd Sem)", details: "Unit 4: Drugs for bronchial asthma (Essay, Sess. II; SA, Sess. I); Bronchodilators (SN, Oct 2022; SN, Sep 2014; etc.)." },
        { subject: "Pathology I (3rd Sem)", details: "Unit I: Define Neoplasia (Essay, Oct 2022; Essay, Mar 2014); Differences between benign & malignant tumors (SN, Aug 2024; Essay, Oct 2022)." },
        { subject: "Med-Surg", details: "Unit 1 (ENT): Management of Epistaxis (SN, Aug 2024; MCQ, May 2024); Nursing care for tonsillectomy (SN, Oct 2011)." },
        { subject: "Professionalism", details: "Unit I: Professional Conduct, Etiquettes & behaviors (SN, Sess. I); SOLER technique (VSN, Sess. II)." },
        { subject: "Genetics", details: "Unit II: Mendelian theory of inheritance; Multiple alleles and blood groups." },
        { subject: "Pharmacology I (3rd Sem)", details: "Unit 4: Expectorants, Antitussives (Codeine), Mucolytics; Name four drugs used in cough (SA, Feb 2014)." },
        { subject: "Pathology I (3rd Sem)", details: "Unit I: Cancer cell features; Carcinoma in situ; Tumor metastasis mechanism & routes (Essay, Sep 2018)." }
    ]},
    { day: 6, week: 1, title: "THE PURGE", tasks: [
        { subject: "Pharmacology I (3rd Sem)", details: "Unit 5: Haematinics & treatment of anemia; Oral iron preparations (VSN, Sess. I); Adrenergic Drugs for CHF & vasodilators." },
        { subject: "Pathology I (3rd Sem)", details: "Unit I: Circulatory disturbances: Thrombosis, embolism, shock; Fate of a thrombus (SN, Oct 2022; SN, Sep 2018)." },
        { subject: "Med-Surg", details: "Unit 1 (ENT): Otosclerosis (SN, Oct 2023; SN, Jul 2024); Impacted wax (SN, Oct 2011); Weber's test (MCQ, May 2024)." },
        { subject: "Professionalism", details: "Unit I: Professional Boundaries (MCQ, Aug 2024); Boundary violations (VSN, Sess. I)." },
        { subject: "Genetics", details: "Unit II: Sex-linked inheritance; MCQ on X-linked disorders (Apr 2025)." },
        { subject: "Pharmacology I (3rd Sem)", details: "Unit 5: SN on Digoxin (Oct 2022); Drug therapy in MI (SN, Aug 2013); Antianginals (SN, Sep 2015)." },
        { subject: "Pathology I (3rd Sem)", details: "Unit I: Air embolism (SN, Sep 2018); Fat emboli (MCQ, Sep 2017); Petechiae (MCQ, Oct 2010)." }
    ]},
    { day: 7, week: 1, title: "THE PURGE", tasks: [
        { subject: "Pharmacology I (3rd Sem)", details: "Unit 5: Classify antihypertensive drugs (Essay, Feb 2014); MOA of ACE inhibitors & Beta-blockers (SA, Sess. I; Essay, Apr 2015)." },
        { subject: "Pathology I (3rd Sem)", details: "Unit I: Edema; Transudates and Exudates (SN, Jun 2024); Renal edema (SN, Aug 2013); Lymphedema (SN, Apr 2013)." },
        { subject: "Med-Surg", details: "Unit 2 (Eye): Essay on Glaucoma: definition, types, classification, pathophysiology, management (Essay, Oct 2024; Essay, Sep 2015; Essay, Apr 2025)." },
        { subject: "Professionalism", details: "Unit I: Roles of INC, State Councils, TNAI, SNA, ICN (SN, Sess. I); Code of professional conduct for Nurses in India (SN, Aug 2024)." },
        { subject: "Genetics", details: "Unit II: Mechanism of inheritance; Errors in transmission (mutation); VSN on two inborn errors of metabolism (Apr 2025)." },
        { subject: "Pharmacology I (3rd Sem)", details: "Unit 5: Coagulants & Anticoagulants (SN, Mar 2014); Warfarin; Antiplatelets (SN, Aug 2014); Thrombolytics." },
        { subject: "Pathology I (3rd Sem)", details: "Unit II (Respiratory): Pulmonary infections: Pneumonia (SN, Feb 2014), Lung abscess, Pulmonary tuberculosis (SN, Sep 2018)." }
    ]},
    { day: 8, week: 2, title: "FORCED MARCH", tasks: [
        { subject: "Pharmacology I (3rd Sem)", details: "Unit 6: Insulin & oral hypoglycemics (SN, Feb 2014; SA, Aug 2012); Metformin (SN, Feb 2013)." },
        { subject: "Pathology I (3rd Sem)", details: "Unit II (Respiratory): Ghon complex (SN, Feb 2013); Primary complex (SN, Oct 2010); Lab diagnosis of TB (SN, Sep 2018)." },
        { subject: "Med-Surg", details: "Unit 2 (Eye): Essay on Cataract: definition, types, pathophysiology, diagnostic eval, post-op nursing care plan (Essay, Mar 2016; Essay, Sep 2017; Essay, Oct 2010)." },
        { subject: "Professionalism", details: "Unit II: Define Values, value clarification (Essay, Aug 2024); Personal vs. Professional values (SN, Sess. I)." },
        { subject: "Genetics", details: "Unit III: Maternal, prenatal & genetic influences on development; Conditions affecting the mother: genetic & infections; Consanguinity." },
        { subject: "Pharmacology I (3rd Sem)", details: "Unit 6: Thyroid and anti-thyroid drugs (SN, Apr 2015); Steroids." },
        { subject: "Pathology I (3rd Sem)", details: "Unit II (Respiratory): COPD (Essay, Aug 2016); Chronic bronchitis, Emphysema, Bronchial Asthma (Essay, Sep 2014), Bronchiectasis (SN, Feb 2011)." }
    ]},
    { day: 9, week: 2, title: "FORCED MARCH", tasks: [
        { subject: "Pharmacology I (3rd Sem)", details: "Unit 8: General Principles of Antimicrobials; Classify antibiotics (Essay, Aug 2024; Essay, Sep 2018)." },
        { subject: "Pathology I (3rd Sem)", details: "Unit II (CVS): Atherosclerosis; Ischemia and Infarction; Coronary Block (SN, Apr 2013)." },
        { subject: "Med-Surg", details: "Unit 2 (Eye): Retinal detachment (SN, Mar 2016; SN, Oct 2011; SN, Apr 2025)." },
        { subject: "Professionalism", details: "Unit II: Professional Socialization (VSN, Sess. I); Integration of values; Importance of professional values in nursing (Essay, Sess. I)." },
        { subject: "Genetics", details: "Unit III: MCQ on risk factors in pregnancy (Sep 2018); VSN on two genetic screening tests for congenital abnormalities (Apr 2025)." },
        { subject: "Pharmacology I (3rd Sem)", details: "Unit 8: Penicillin, Cephalosporins (SN, Apr 2015), Aminoglycosides, Macrolides, broad-spectrum antibiotics, Sulfonamides, Quinolones." },
        { subject: "Pathology I (3rd Sem)", details: "Unit II (CVS): Rheumatic Heart Disease (SN, Feb 2010); Aschoff nodules (MCQ, Aug 2016); Infective endocarditis." }
    ]},
    { day: 10, week: 2, title: "FORCED MARCH", tasks: [
        { subject: "Pharmacology I (3rd Sem)", details: "Unit 8: Antitubercular drugs (Rifampicin); Antileprosy drugs (Dapsone)." },
        { subject: "Pathology I (3rd Sem)", details: "Unit II (GIT): Peptic ulcer disease (SN, Feb 2014); Gastritis-H Pylori infection (MCQ, Aug 2016)." },
        { subject: "Med-Surg", details: "Unit 2 (Eye): Conjunctivitis (MCQ, Aug 2024); Blepharitis (MCQ, Apr 2025); Stye (SA, Oct 2023); Trachoma (SAFE strategy, MCQ, Mar 2016)." },
        { subject: "Professionalism", details: "Unit II: Specific values: Caring (SN, Sess. II), Compassion, Sympathy vs. Empathy (MCQ, Sess I), Altruism (MCQ, Aug 2024)." },
        { subject: "Genetics", details: "Full Review. You should know this pitifully small subject by now. Review every topic and question again. Don't be stupid." },
        { subject: "Pharmacology I (3rd Sem)", details: "Unit 8: Antimalarials (Chloroquine); Antiviral agents (Acyclovir); Antihelminthics; Antiscabies agents." },
        { subject: "Pathology I (3rd Sem)", details: "Unit II (GIT): Oral Leukoplakia; Squamous cell carcinoma (Essay, Oct 2010); Esophageal cancer; Gastric cancer." }
    ]},
    { day: 11, week: 2, title: "FORCED MARCH", tasks: [
        { subject: "Pharmacology II (4th Sem)", details: "Unit 1: Antihistamines (SN, Aug 2024); Topical applications for eye, ear, nose (Chloramphenicol, Pilocarpine)." },
        { subject: "Pathology I (3rd Sem)", details: "Unit II (GIT): Typhoid ulcer (SN, Sep 2015); Inflammatory Bowel Disease (Crohn's, Ulcerative colitis)." },
        { subject: "Med-Surg", details: "Unit 3 (Renal): Essay on Acute Glomerulonephritis (AGN): causes, manifestations, nursing care (Essay, Sep 2017)." },
        { subject: "Professionalism", details: "Unit II: Conscientiousness; Dedication to work; Respect for human dignity; Privacy and confidentiality." },
        { subject: "Pharmacology II (4th Sem)", details: "Unit 2: Diuretics and antidiuretics (Essay, Jul 2024); Complications of diuretic therapy (SN, Feb 2011)." },
        { subject: "Pathology I (3rd Sem)", details: "Unit II (Liver, GB, Pancreas): Hepatitis; Amoebic Liver abscess; Cirrhosis of Liver (SN, Aug 2016; Essay, Apr 2015)." },
        { subject: "Med-Surg", details: "Unit 3 (Renal): Essay on Acute Renal Failure (ARF): definition, causes, pathophysiology, clinical manifestation, diagnostic eval, management (Essay, May 2024; Essay, Apr 2025)." }
    ]},
    { day: 12, week: 2, title: "FORCED MARCH", tasks: [
        { subject: "Pharmacology II (4th Sem)", details: "Unit 2: Urinary antiseptics (SN, Aug 2024); Name 4 urinary antiseptics (VSN, Apr 2025)." },
        { subject: "Pathology I (3rd Sem)", details: "Unit II (Liver, GB, Pancreas): Cholecystitis; Pancreatitis; Tumors of liver, Gall bladder and Pancreas." },
        { subject: "Med-Surg", details: "Unit 3 (Renal): SN on Dialysis (Jul 2024); Complications of haemodialysis (SN, Sep 2017)." },
        { subject: "Professionalism", details: "Unit II: Honesty, integrity, truth-telling (veracity); Trust, credibility, fidelity; Advocacy for patients, work environment, profession." },
        { subject: "Pharmacology II (4th Sem)", details: "Unit 3: Analgesics: NSAIDs (Essay, Aug 2024; SN, Oct 2022); Paracetamol (Essay, Sep 2017)." },
        { subject: "Pathology I (3rd Sem)", details: "Unit II (Skeletal): Bone healing; Osteoporosis; Osteomyelitis (SN, Oct 2010); Tumors (Osteoma, MCQ, Sep 2015)." },
        { subject: "Med-Surg", details: "Unit 3 (Renal): Management of UTI (SN, Oct 2024); Nephrotic syndrome treatment goals (MCQ, Apr 2025)." }
    ]},
    { day: 13, week: 2, title: "FORCED MARCH", tasks: [
        { subject: "Pharmacology II (4th Sem)", details: "Unit 3: Opioids & other central analgesics; Morphine (SN, Apr 2025); Fentanyl (VSN, Jul 2024)." },
        { subject: "Pathology I (3rd Sem)", details: "Unit II (Skeletal): Arthritis - Rheumatoid arthritis and Osteoarthritis." },
        { subject: "Med-Surg", details: "Unit 3 (Renal): Management of renal calculi (SN, Aug 2024); Lithotripsy (MCQ, May 2024)." },
        { subject: "Professionalism", details: "Unit II: Patient's rights (SN, Sess. II)." },
        { subject: "Pharmacology II (4th Sem)", details: "Unit 3: General & local anesthetics (Essay, Oct 2010); Lignocaine (SN, Aug 2012); Thiopentone sodium (SN, Feb 2010)." },
        { subject: "Pathology I (3rd Sem)", details: "Unit II (Endocrine): Diabetes Mellitus: classification, lab diagnosis, complications (Essay, Aug 2013); Goitre; Carcinoma thyroid." },
        { subject: "Med-Surg", details: "Unit 3 (Renal): Benign prostate hypertrophy (BPH) (SN, Oct 2024; SN, Jul 2024)." }
    ]},
    { day: 14, week: 2, title: "FORCED MARCH", tasks: [
        { subject: "Pharmacology II (4th Sem)", details: "Unit 3: Hypnotics and sedatives (SN, Sep 2014); Diazepam (SN, Apr 2025)." },
        { subject: "Pathology I (3rd Sem)", details: "Unit III (Hematology): Blood tests: Hemoglobin, WBC & platelet counts (SN, Sep 2014), PCV, ESR." },
        { subject: "Med-Surg", details: "Unit 4 (Male Genital): Toxic shock Syndrome (SA, Oct 2024); Gynecomastia (MCQ, May 2024); Define Infertility (VSN, May 2024)." },
        { subject: "Professionalism", details: "Unit III: Define Ethics, Bioethics, and Ethical Principles (Essay, Aug 2024)." },
        { subject: "Pharmacology II (4th Sem)", details: "Unit 3: Antidepressants; SSRIs (SN, Jun 2024); Tricyclic anti-depressants (Amitriptyline)." },
        { subject: "Pathology I (3rd Sem)", details: "Unit III (Hematology): Coagulation tests: BT, PT, APTT; Blood coagulation factors (Essay, Apr 2015)." },
        { subject: "Med-Surg", details: "Unit 5 (Burns): Define Burns & types (Essay, Oct 2011); Pathophysiology of burns (Essay, Oct 2023)." }
    ]},
    { day: 15, week: 3, title: "THE GRIND", tasks: [
        { subject: "Pharmacology II (4th Sem)", details: "Unit 3: Anticonvulsants (SN, Feb 2014); Phenytoin (Essay, Apr 2025); Valproic acid (Essay, Jul 2024)." },
        { subject: "Pathology I (3rd Sem)", details: "Unit III (Hematology): Blood grouping & cross matching; Blood components; Plasmapheresis; Transfusion reactions." },
        { subject: "Med-Surg", details: "Unit 5 (Burns): Emergency management of burns (Essay, Oct 2010); Fluid replacement (Brooke army formula, SA, Oct 2024; Parkland formula, MCQ, Jun 2024)." },
        { subject: "Professionalism", details: "Unit III: Beneficence, Non-maleficence (patient safety, protecting from harm, reporting errors), Justice (fairness, equality, equitable access), Autonomy (self-determination)." },
        { subject: "Pharmacology II (4th Sem)", details: "Unit 3: Drugs for neurodegenerative disorders (Levodopa/Carbidopa); Stimulants, ethyl alcohol and treatment of methyl alcohol poisoning (Ethanol/Fomepizole)." },
        { subject: "Pathology II (4th Sem)", details: "Unit I (Renal): Glomerulonephritis (SN, Apr 2025); Pyelonephritis (SN, Jul 2024); Renal calculi (Essay, Aug 2024)." },
        { subject: "Med-Surg", details: "Unit 5 (Burns): Rule of Nine (SN, Oct 2024; SA, Oct 2023); Assessment of Burns (SN, Aug 2024); Types of skin grafts (SA, Oct 2024)." }
    ]},
    { day: 16, week: 3, title: "THE GRIND", tasks: [
        { subject: "Pharmacology II (4th Sem)", details: "Unit 4: Estrogens and progesterones; Oral contraceptives (Essay, Apr 2025); Hormone replacement therapy." },
        { subject: "Pathology II (4th Sem)", details: "Unit I (Renal): Cystitis; Renal Cell Carcinoma; Renal Failure (Acute and Chronic)." },
        { subject: "Med-Surg", details: "Unit 6 (Neuro): Essay on CVA: etiology, pathophysiology, clinical manifestations, comprehensive management (Essay, Oct 2010; Essay, Oct 2023; Essay, Jul 2024)." },
        { subject: "Professionalism", details: "Unit III: Informed Consent: definition & nurse's role (SN, Aug 2024); Ethical liabilities." },
        { subject: "Pharmacology II (4th Sem)", details: "Unit 4: Vaginal contraceptives; Drugs for infertility (Clomiphene citrate); Medical termination of pregnancy (Misoprostol)." },
        { subject: "Pathology II (4th Sem)", details: "Unit I (Male Genital): Cryptorchidism; Testicular atrophy; Prostatic hyperplasia (BPH, SN, Apr 2025); Carcinoma penis (SN, Jul 2024) and Prostate." },
        { subject: "Med-Surg", details: "Unit 6 (Neuro): Essay on Head Injury: types, health assessment, nursing care plan, management of increased ICP (Essay, Oct 2008; Essay, Sep 2015; Essay, Aug 2024)." }
    ]},
    { day: 17, week: 3, title: "THE GRIND", tasks: [
        { subject: "Pharmacology II (4th Sem)", details: "Unit 5 & 6: Uterine stimulants (Oxytocin) and relaxants; Drugs for pregnant women; CPR and emergency drugs (Adrenaline, etc.) (Essay, Mar 2014)." },
        { subject: "Pathology II (4th Sem)", details: "Unit I (Female Genital): Carcinoma cervix (SN, Sep 2018; Essay, Aug 2016); Carcinoma of endometrium (SN, Jul 2024)." },
        { subject: "Med-Surg", details: "Unit 6 (Neuro): Essay on Meningitis: causes, pathophysiology, signs, medical and nursing management (Essay, Mar 2016)." },
        { subject: "Professionalism", details: "Unit III: Ethical dilemmas: common problems, conflict of interest, paternalism, deception (VSN, Aug 2024), privacy, valid consent/refusal, allocation of resources, whistle-blowing." },
        { subject: "Pharmacology II (4th Sem)", details: "Unit 6: IV fluids & electrolytes replacement (SN, Mar 2014); Common poisons & antidotes (OPC, lead, methanol)." },
        { subject: "Pathology II (4th Sem)", details: "Unit I (Female Genital): Uterine fibroids (SN, Sep 2015); Vesicular mole and Choriocarcinoma; Ovarian cyst and tumors (Pseudomucinous cystadenoma)." },
        { subject: "Med-Surg", details: "Unit 6 (Neuro): Essay on Seizures: nursing assessment, counseling, nursing care plan (Essay, Oct 2008; Essay, Oct 2024)." }
    ]},
    { day: 18, week: 3, title: "THE GRIND", tasks: [
        { subject: "Pharmacology II (4th Sem)", details: "Unit 6: Vitamins (Fat soluble, Vit D, Vit C, Vit B1) and minerals supplementation; Vaccines & sera." },
        { subject: "Pathology II (4th Sem)", details: "Unit I (Breast): Fibrocystic changes; Fibroadenoma (SN, Apr 2025); Carcinoma of the Breast (Essay, Oct 2010)." },
        { subject: "Med-Surg", details: "Unit 6 (Neuro): Parkinson's disease (SN, Oct 2023); GBS (SN, Oct 2024); Multiple sclerosis (SN, Oct 2011); Alzheimer's disease (SN, Oct 2010)." },
        { subject: "Professionalism", details: "Unit III: Beginning of life issues: Abortion, substance abuse, fetal therapy, mandated contraception, infertility treatment." },
        { subject: "Pharmacology II (4th Sem)", details: "Unit 6: Teratogenicity (SA, Aug 2014); Placebo (SA, Oct 2022); Prodrug (MCQ, Feb 2014)." },
        { subject: "Pathology II (4th Sem)", details: "Unit I (CNS): Meningitis (SN, Apr 2025); Encephalitis; Stroke; Tumors of CNS." },
        { subject: "Med-Surg", details: "Unit 7 (Immuno): Universal precautions (SN, Sep 2017); National Aids Control programme (SN, Apr 2025)." }
    ]},
    { day: 19, week: 3, title: "THE GRIND", tasks: [
        { subject: "Pharmacology II (4th Sem)", details: "Unit 7: Anticancer drugs / Chemotherapeutic drugs (Essay, Apr 2013)." },
        { subject: "Pathology II (4th Sem)", details: "Unit II (Clinical Path): Examination of body cavity fluids: CSF (SN, Apr 2025), sputum, wound discharge; Paracentesis." },
        { subject: "Med-Surg", details: "Unit 8 (Oncology): Essay on Leukaemia: types, management (Essay, Oct 2024)." },
        { subject: "Professionalism", details: "Unit III: End-of-life issues: euthanasia (MCQ, Jun 2024), Do Not Resuscitate (DNR), withdrawing life support, determination of brain death, hastening death." },
        { subject: "Pharmacology II (4th Sem)", details: "Unit 7: Immuno-suppressants (SN, Apr 2013) and Immunostimulants." },
        { subject: "Pathology II (4th Sem)", details: "Unit II (Clinical Path): CSF findings in various forms of meningitis (SN, Aug 2024)." },
        { subject: "Med-Surg", details: "Unit 8 (Oncology): Essay on Uterine cancer: risk factors, clinical features, medical/surgical management, pre/post-op care for Hysterectomy (Essay, Oct 2023)." }
    ]},
    { day: 20, week: 3, title: "THE GRIND", tasks: [
        { subject: "Pharmacology II (4th Sem)", details: "Unit 8 & 9: Drugs in alternative systems of medicine; Fundamental principles of prescribing (SN, Aug 2024)." },
        { subject: "Pathology II (4th Sem)", details: "Unit II (Clinical Path): Analysis of semen: sperm count, motility, morphology, liquefaction time (MCQ, Jun 2024)." },
        { subject: "Med-Surg", details: "Unit 8 (Oncology): Essay on cancer of the larynx: prep for laryngectomy, post-op care (Essay, Oct 2011)." },
        { subject: "Professionalism", details: "Unit III: Process of ethical decision making (Essay, Sess. II)." },
        { subject: "Pharmacology I & II Review", details: "Go through the list of 'Frequently Repeated Questions/Topics' in your pharm doc. Read it. Memorize it." },
        { subject: "Pathology II (4th Sem)", details: "Unit II (Clinical Path): Urine: Physical characteristics, Analysis (Benedict's test), Culture, sample collection techniques, urine crystals." },
        { subject: "Med-Surg", details: "Unit 8 (Oncology): Essay on breast cancer: causes, risk factors, pathophysiology, management (Essay, Sep 2017)." }
    ]},
    { day: 21, week: 3, title: "THE GRIND", tasks: [
        { subject: "Pharmacology I Full Review", details: "Re-read and answer every question from the 3rd Sem syllabus." },
        { subject: "Pathology II (4th Sem)", details: "Unit II (Clinical Path): Faeces: Characteristics, Stool examination (Occult blood, Ova, Parasite, Cyst), Steatorrhea (MCQ, Jul 2024)." },
        { subject: "Med-Surg", details: "Unit 8 (Oncology): Nurse's role for patients undergoing chemotherapy (Essay, Mar 2016); Breast self-examination (SN, Mar 2016); Pain management (SN, Mar 2016)." },
        { subject: "Professionalism", details: "Unit III: Ethics committee: Roles and responsibilities (SN, Sess. II); Patients' Bill of Rights (Rights to referral, records, etc.)." },
        { subject: "Pharmacology II Full Review", details: "Re-read and answer every question from the 4th Sem syllabus." },
        { subject: "Pathology I Full Review", details: "Re-read and answer every question from the 3rd Sem syllabus." },
        { subject: "Med-Surg", details: "Unit 9 (Emergency): Essay on disaster nursing: principles, types, preparedness cycle, role of nurse (Essay, Sep 2015)." }
    ]},
    { day: 22, week: 4, title: "JUDGEMENT", tasks: [{ subject: "Review", details: "Pathology II Full Review. Write out the key features of every disease listed. All of them. Med-Surg - Unit 9 (Emergency): Triage (SA, Oct 2024; MCQ, Apr 2025); CPR (SN, Sep 2017); Control haemorrhage; Poisoning (SN, Oct 2010)." }] },
    { day: 23, week: 4, title: "JUDGEMENT", tasks: [{ subject: "Review", details: "Med-Surg Full Review. Write a one-page summary for the nursing management of the top 5 essay questions: CVA, Head Injury, Burns, Glaucoma, ARF. Professionalism Full Review. Write down every definition from memory. Then check. Do it again." }] },
    { day: 24, week: 4, title: "JUDGEMENT", tasks: [{ subject: "Review", details: "Pharmacology I & II - Combined Review. Write down every drug classification you can remember. For each class, write one prototype drug, one major use, and one fatal side effect. Check your work. The parts you got wrong, write out 10 times." }] },
    { day: 25, week: 4, title: "JUDGEMENT", tasks: [{ subject: "Review", details: "Pathology I & II - Combined Review. From memory, list 5 key pathological features for each organ system (Respiratory, CVS, GIT, Renal, CNS). Then, write down the normal values for all hematological and clinical path tests." }] },
    { day: 26, week: 4, title: "JUDGEMENT", tasks: [{ subject: "Active Recall", details: "Active Recall Day 1: Take blank paper. For Pharmacology, write down everything you know about Unit 1-4 (3rd Sem). For Pathology, do the same for Unit I (Gen Path). For Med-Surg, do Unit 1-3. Do not look at your notes until you are finished. Grade yourself brutally." }] },
    { day: 27, week: 4, title: "JUDGEMENT", tasks: [{ subject: "Active Recall", details: "Active Recall Day 2: Repeat yesterday's exercise for Pharmacology Units 5-8 (3rd Sem), Pathology Unit II (Special Path), and Med-Surg Units 4-6." }] },
    { day: 28, week: 4, title: "JUDGEMENT", tasks: [{ subject: "Active Recall", details: "Active Recall Day 3: Repeat for Pharmacology (4th Sem, Units 1-4), Pathology (4th Sem, Unit I), and Med-Surg Units 7-11." }] },
    { day: 29, week: 4, title: "JUDGEMENT", tasks: [{ subject: "Active Recall", details: "Active Recall Day 4: Repeat for Pharmacology (4th Sem, Units 5-9), Pathology (4th Sem, Unit II), Professionalism & Genetics (all units). By now, you should be scoring above 90% on your recall. If not, you know what to do. Repeat the failed sections." }] },
    { day: 30, week: 4, title: "JUDGEMENT", tasks: [{ subject: "Final Gauntlet", details: "Answer every single Multiple Choice Question from all the PDFs in one sitting. Then answer every Very Short Note/Short Answer question. Finally, outline the essays for the top 10 most frequently repeated topics. This is your dress rehearsal for the pain of the exam. Do not stop until you are done." }] },
];

let appState = {
    currentDay: 1,
    completionStatus: {}
};

let overallProgressChart, weeklyProgressChart, countdownChart;
let intersectionObserver;
let countdownInterval;

function saveState() {
    try {
        localStorage.setItem('studyPlanProgress_v1', JSON.stringify(appState.completionStatus));
    } catch (e) {
        console.error("Failed to save state to localStorage:", e);
    }
}

function loadState() {
    try {
        const savedState = localStorage.getItem('studyPlanProgress_v1');
        const initialState = {};
        planData.forEach((day) => {
            day.tasks.forEach((task, taskIndex) => {
                const taskId = `day-${day.day}-task-${taskIndex}`;
                initialState[taskId] = false;
            });
        });
        
        if (savedState) {
            appState.completionStatus = { ...initialState, ...JSON.parse(savedState) };
        } else {
            appState.completionStatus = initialState;
        }
    } catch (e) {
        console.error("Failed to load state from localStorage:", e);
    }
}

function renderDaySelector() {
    const selectorContainer = document.getElementById('day-selector');
    selectorContainer.innerHTML = ''; 

    const weeks = {};
    planData.forEach(day => {
        if (!weeks[day.week]) {
            weeks[day.week] = {
                title: day.title,
                days: []
            };
        }
        weeks[day.week].days.push(day.day);
    });

    for (const weekNumber in weeks) {
        const weekData = weeks[weekNumber];
        
        const weekContainer = document.createElement('div');
        weekContainer.className = 'mb-4';

        const weekHeader = document.createElement('button');
        weekHeader.className = 'w-full text-left text-lg font-bold text-primary mb-2 flex justify-between items-center'; 
        weekHeader.innerHTML = `<span>Week ${weekNumber}: ${weekData.title}</span><svg class="w-5 h-5 transition-transform" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7" /></svg>`;
        
        const dayGrid = document.createElement('div');
        dayGrid.className = 'accordion-content grid grid-cols-4 sm:grid-cols-5 md:grid-cols-7 lg:grid-cols-4 gap-2';

        weekData.days.forEach(dayNum => {
            const dayButton = document.createElement('button');
            dayButton.textContent = dayNum;
            dayButton.dataset.day = dayNum;
            dayButton.className = 'day-selector-item p-2 aspect-square flex items-center justify-center rounded-xl text-xs font-medium text-center shadow-sm';
            dayGrid.appendChild(dayButton);
        });
        
        weekContainer.appendChild(weekHeader);
        weekContainer.appendChild(dayGrid);
        selectorContainer.appendChild(weekContainer);

        weekHeader.addEventListener('click', () => {
            dayGrid.classList.toggle('open');
            weekHeader.querySelector('svg').classList.toggle('rotate-180');
        });

        // Open the current week by default
        if (weekData.days.includes(appState.currentDay)) {
            dayGrid.classList.add('open');
            weekHeader.querySelector('svg').classList.add('rotate-180');
        }
    }
}

function renderAllPlans() {
    const detailsContainer = document.getElementById('plan-details');
    detailsContainer.innerHTML = '';

    planData.forEach((dayData, dayIndex) => {
        const daySection = document.createElement('div');
        daySection.id = `day-plan-${dayData.day}`;
        daySection.className = 'day-plan-section pt-8 -mt-8';
        
        let tasksHtml = '';
        dayData.tasks.forEach((task, index) => {
            const taskId = `day-${dayData.day}-task-${index}`;
            const isCompleted = appState.completionStatus[taskId] || false;
            tasksHtml += `
                <div class="task-card p-4 rounded-xl shadow-sm flex items-start gap-4 ${isCompleted ? 'completed' : 'pending'} stagger-load-item">
                    <div>
                        <input type="checkbox" id="${taskId}" data-day="${dayData.day}" data-task-index="${index}" class="mt-1 h-5 w-5 rounded border-slate-300 text-blue-500 focus:ring-blue-500 cursor-pointer" ${isCompleted ? 'checked' : ''}>
                    </div>
                    <div>
                        <label for="${taskId}" class="font-bold text-primary">${task.subject}</label>
                        <p class="text-secondary text-sm mt-1">${task.details}</p>
                    </div>
                </div>
            `;
        });

        daySection.innerHTML = `
            <div class="p-1 mb-10">
                <h2 class="text-2xl font-black text-primary sticky top-0 py-2 z-10" style="background-color: rgba(30, 41, 59, 0.8); backdrop-filter: blur(20px); -webkit-backdrop-filter: blur(20px);">Day ${dayData.day}: Daily Tasks</h2>
                <div class="mt-6 space-y-4">${tasksHtml}</div>
            </div>
        `;
        detailsContainer.appendChild(daySection);
    });
}

function setActiveDay(dayNumber) {
    appState.currentDay = dayNumber;
    document.querySelectorAll('.day-selector-item').forEach(btn => {
        btn.classList.remove('active');
        if (parseInt(btn.dataset.day) === dayNumber) {
            btn.classList.add('active');
        }
    });
}

function setupScrollSpy() {
    if (intersectionObserver) {
        intersectionObserver.disconnect();
    }

    const options = {
        root: document.getElementById('main-content-wrapper'),
        rootMargin: '0px 0px -50% 0px', /* Adjusted for better scroll-spy accuracy */
        threshold: 0
    };

    intersectionObserver = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
            if (entry.isIntersecting) {
                const day = parseInt(entry.target.id.split('-')[2]);
                setActiveDay(day);
            }
        });
    }, options);

    document.querySelectorAll('.day-plan-section').forEach(section => {
        intersectionObserver.observe(section);
    });
}

function calculateProgress() {
    const totalTasks = planData.reduce((acc, day) => acc + day.tasks.length, 0);
    const completedTasks = Object.values(appState.completionStatus).filter(Boolean).length;
    
    const weeklyCompleted = { week1: 0, week2: 0, week3: 0, week4: 0 };
    const weeklyPending = { week1: 0, week2: 0, week3: 0, week4: 0 };
    const weeklyTotals = { week1: 0, week2: 0, week3: 0, week4: 0 };

    planData.forEach((day) => {
        const weekKey = `week${day.week}`;
        day.tasks.forEach((task, taskIndex) => {
            const taskId = `day-${day.day}-task-${taskIndex}`;
            weeklyTotals[weekKey]++;
            if (appState.completionStatus[taskId]) {
                weeklyCompleted[weekKey]++;
            }
        });
    });

    for(const week in weeklyTotals) {
        weeklyPending[week] = weeklyTotals[week] - weeklyCompleted[week];
    }

    return {
        total: totalTasks,
        completed: completedTasks,
        weeklyCompleted: weeklyCompleted,
        weeklyPending: weeklyPending
    };
}

function renderCharts() {
    const overallCtx = document.getElementById('overallProgressChart').getContext('2d');
    const weeklyCtx = document.getElementById('weeklyProgressChart').getContext('2d');
    const progress = calculateProgress();

    if (overallProgressChart) overallProgressChart.destroy();
    overallProgressChart = new Chart(overallCtx, {
        type: 'doughnut',
        data: {
            labels: ['Completed', 'Pending'],
            datasets: [{
                data: [progress.completed, progress.total - progress.completed],
                backgroundColor: ['#4CAF50', 'rgba(148, 163, 184, 0.5)'], /* Material Green, Slate-400 with opacity */
                borderColor: ['rgba(51, 65, 85, 0.6)'], /* Card background color */
                borderWidth: 4,
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            cutout: '70%',
            plugins: { legend: { display: false }, tooltip: { enabled: true } }
        }
    });
    
    if(weeklyProgressChart) weeklyProgressChart.destroy();
    weeklyProgressChart = new Chart(weeklyCtx, {
        type: 'bar',
        data: {
            labels: ['Week 1', 'Week 2', 'Week 3', 'Week 4'],
            datasets: [
                {
                    label: 'Completed',
                    data: Object.values(progress.weeklyCompleted),
                    backgroundColor: '#4CAF50', /* Material Green */
                    borderRadius: 4,
                },
                {
                    label: 'Pending',
                    data: Object.values(progress.weeklyPending),
                    backgroundColor: 'rgba(148, 163, 184, 0.5)', /* Slate-400 with opacity */
                    borderRadius: 4,
                }
            ]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            scales: {
                y: { 
                    stacked: true, 
                    beginAtZero: true, 
                    grid: { color: 'rgba(255, 255, 255, 0.1)' }, /* Lighter grid lines */
                    ticks: { precision: 0, color: '#cbd5e1' } /* Secondary text color */
                },
                x: { 
                    stacked: true, 
                    grid: { display: false },
                    ticks: { color: '#cbd5e1' } /* Secondary text color */
                }
            },
            plugins: { 
                legend: { 
                    position: 'bottom', 
                    labels: { color: '#cbd5e1' } /* Secondary text color for legend labels */
                }, 
                tooltip: { enabled: true, mode: 'index' } 
            }
        }
    });
}

function startCountdown() {
    const countdownDate = new Date("2025-08-18T10:00:00+05:30").getTime(); // Exam date and time (IST)
    const startDate = new Date("2025-07-01T00:00:00+05:30").getTime(); // Starting point for progress bar

    const totalDuration = countdownDate - startDate;

    const countdownCtx = document.getElementById('countdownChart').getContext('2d');
    if (countdownChart) countdownChart.destroy();
    countdownChart = new Chart(countdownCtx, {
        type: 'doughnut',
        data: {
            datasets: [{
                data: [0, 100],
                backgroundColor: ['#3b82f6', 'rgba(148, 163, 184, 0.4)'], /* Primary Blue, Slate-400 with opacity */
                borderColor: ['rgba(51, 65, 85, 0.6)'],
                borderWidth: 4,
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            cutout: '80%',
            plugins: { legend: { display: false }, tooltip: { enabled: false } },
            events: []
        }
    });

    if (countdownInterval) clearInterval(countdownInterval);

    countdownInterval = setInterval(() => {
        const currentRealTime = new Date().getTime(); // Use current real time for live update
        const distance = countdownDate - currentRealTime;

        if (distance < 0) {
            clearInterval(countdownInterval);
            document.getElementById("countdown-text").innerHTML = "EXAM DAY";
            countdownChart.data.datasets[0].data = [100, 0];
            countdownChart.update('none');
            return;
        }

        const days = Math.floor(distance / (1000 * 60 * 60 * 24));
        const hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
        const minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
        const seconds = Math.floor((distance % (1000 * 60)) / 1000);

        document.getElementById("days").innerText = String(days).padStart(2, '0');
        document.getElementById("hours").innerText = String(hours).padStart(2, '0');
        document.getElementById("minutes").innerText = String(minutes).padStart(2, '0');
        document.getElementById("seconds").innerText = String(seconds).padStart(2, '0');
        
        const elapsed = currentRealTime - startDate;
        const elapsedPercent = (elapsed / totalDuration) * 100;
        countdownChart.data.datasets[0].data = [elapsedPercent, 100 - elapsedPercent];
        countdownChart.update('none');

    }, 1000);
}

function filterContent(type) {
    const theory = document.querySelector('#view-timetable .theory');
    const practical = document.querySelector('#view-timetable .practical');

    if (theory && practical) {
        if (type === 'theory') {
            theory.style.display = 'block';
            practical.style.display = 'block';
        } else if (type === 'practical') {
            theory.style.display = 'none';
            practical.style.display = 'block';
        }
    }
}

function scrollToExam(id, type) {
    switchView('view-timetable');
    setTimeout(() => {
        filterContent(type);
        setTimeout(() => {
            const element = document.getElementById(id);
            if (element) {
                element.scrollIntoView({ behavior: 'smooth', block: 'center' }); /* Smooth scroll to center */
            }
        }, 100);
    }, 50);
}

function switchView(viewId) {
    document.querySelectorAll('#main-content > div').forEach(view => view.classList.add('hidden'));
    document.querySelectorAll('#top-tab-nav .tab').forEach(item => item.classList.remove('active'));
    
    document.getElementById(viewId).classList.remove('hidden');
    
    const activeTab = document.querySelector(`#top-tab-nav .tab[data-view="${viewId}"]`);
    if(activeTab) activeTab.classList.add('active');

    if (viewId === 'view-progress') {
        renderCharts();
    } else if (viewId === 'view-timetable') {
        // Ensure all timetable content is visible when switching to this view
        const theoryDiv = document.querySelector('#view-timetable .theory');
        const practicalDiv = document.querySelector('#view-timetable .practical');
        if (theoryDiv) theoryDiv.style.display = 'block';
        if (practicalDiv) practicalDiv.style.display = 'block';
    }

    // Apply staggered load animation for the newly active view's content
    const currentViewContent = document.getElementById(viewId);
    if (currentViewContent) {
        const staggerItems = currentViewContent.querySelectorAll('.stagger-load-item');
        staggerItems.forEach((item, index) => {
            // Reset 'loaded' class to ensure animation plays again if view is re-activated
            item.classList.remove('loaded'); 
            // Force browser to re-calculate style to ensure animation restarts from the initial state (opacity:0, translateY:20px)
            void item.offsetWidth; 
            setTimeout(() => {
                item.classList.add('loaded');
            }, index * 100); // Stagger by 100ms per item
        });
    }
}

document.addEventListener('DOMContentLoaded', () => {
    // Corrected initialization order
    loadState();
    renderDaySelector();
    renderAllPlans(); // Render content first
    switchView('view-mission-schedule'); // Then switch view to apply animations
    setActiveDay(appState.currentDay);
    setupScrollSpy();
    startCountdown();

    const daySelector = document.getElementById('day-selector');
    const mainContentWrapper = document.getElementById('main-content-wrapper');
    const backToTopBtn = document.getElementById('backToTopBtn');

    daySelector.addEventListener('click', (e) => {
        const button = e.target.closest('button.day-selector-item');
        if (button) {
            const day = parseInt(button.dataset.day, 10);
            const targetElement = document.getElementById(`day-plan-${day}`);
            if (targetElement) {
                targetElement.scrollIntoView({ behavior: 'smooth', block: 'center' }); /* Smooth scroll to center */
            }
        }
    });

    document.getElementById('plan-details').addEventListener('change', (e) => {
        if (e.target.matches('input[type="checkbox"]')) {
            const taskId = e.target.id;
            appState.completionStatus[taskId] = e.target.checked;
            
            const card = e.target.closest('.task-card');
            if (e.target.checked) {
                card.classList.add('completed');
                card.classList.remove('pending');
            } else {
                card.classList.remove('completed');
                card.classList.add('pending');
            }
            saveState();
        }
    });

    document.getElementById('top-tab-nav').addEventListener('click', (e) => {
        const navItem = e.target.closest('.tab');
        if (navItem) {
            switchView(navItem.dataset.view);
        }
    });

    mainContentWrapper.addEventListener('scroll', () => {
        if (mainContentWrapper.scrollTop > 300) {
            backToTopBtn.classList.add('show');
        } else {
            backToTopBtn.classList.remove('show');
        }
    });

    backToTopBtn.addEventListener('click', () => {
        mainContentWrapper.scrollTo({
            top: 0,
            behavior: 'smooth'
        });
    });
});
</script>

</body>
</html>
