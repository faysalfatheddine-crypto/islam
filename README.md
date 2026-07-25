<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Islam Kids</title>
    <meta name="theme-color" content="#2EC4B6">
    <style>
        /* =========================================
           CSS: التنسيقات والألوان والحركات
           ========================================= */
        :root {
            --primary: #2EC4B6;
            --primary-dark: #188f82;
            --secondary: #FF9F1C;
            --accent: #FFD166;
            --light: #FDFFFC;
            --dark: #011627;
            --bg: #0a2540;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        body { background-color: var(--bg); color: var(--light); overflow-x: hidden; user-select: none; }
        .hidden { display: none !important; }

        /* الحركات (Animations) */
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        @keyframes fadeOut { from { opacity: 1; } to { opacity: 0; } }
        @keyframes float { 0% { transform: translateY(0px); } 50% { transform: translateY(-10px); } 100% { transform: translateY(0px); } }
        @keyframes pulse { 0% { transform: scale(1); } 50% { transform: scale(1.05); } 100% { transform: scale(1); } }
        @keyframes cloudMove { from { transform: translateX(100vw); } to { transform: translateX(-100%); } }
        @keyframes twinkle { 0%, 100% { opacity: 0.2; } 50% { opacity: 1; } }

        .fade-in { animation: fadeIn 0.5s ease forwards; }
        .fade-out { animation: fadeOut 0.5s ease forwards; }

        /* شاشة البداية (Splash Screen) */
        .splash-screen {
            position: fixed; top: 0; left: 0; width: 100vw; height: 100vh;
            background: linear-gradient(135deg, var(--primary), var(--primary-dark));
            display: flex; justify-content: center; align-items: center; z-index: 9999;
            transition: opacity 0.5s ease;
        }
        .logo-container { text-align: center; }
        .splash-logo { animation: pulse 2s infinite; margin-bottom: 20px; }
        .splash-title { font-size: 2.5rem; color: var(--light); font-weight: bold; text-shadow: 2px 2px 4px rgba(0,0,0,0.2); }

        /* خلفية السماء والمسجد */
        .sky-bg {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%; z-index: -1;
            background: linear-gradient(to bottom, #0a2540 0%, #1a4a76 100%);
        }
        .stars { 
            position: absolute; width: 100%; height: 100%; 
            background: radial-gradient(2px 2px at 20px 30px, #eee, rgba(0,0,0,0)), 
                        radial-gradient(2px 2px at 40px 70px, #fff, rgba(0,0,0,0)), 
                        radial-gradient(2px 2px at 50px 160px, #ddd, rgba(0,0,0,0)); 
            background-repeat: repeat; animation: twinkle 4s infinite; 
        }
        .clouds { position: absolute; top: 10%; left: 0; width: 100%; height: 50px; opacity: 0.6; }
        .cloud { position: absolute; background: white; border-radius: 50px; width: 100px; height: 30px; animation: cloudMove 20s linear infinite; }
        .cloud::before, .cloud::after { content: ''; position: absolute; background: white; border-radius: 50%; }
        .cloud::before { width: 40px; height: 40px; top: -15px; left: 15px; }
        .cloud::after { width: 50px; height: 50px; top: -25px; left: 40px; }
        .mosque-bg { 
            position: absolute; bottom: 0; width: 100%; height: 200px; 
            background: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1000 200"><path d="M0,200 L1000,200 L1000,150 Q950,100 900,150 T800,150 T700,150 T600,100 T500,150 T400,150 T300,100 T200,150 T100,150 Q50,100 0,150 Z" fill="%231a4a76" opacity="0.5"/></svg>') no-repeat bottom center; 
            background-size: cover; 
        }

        /* محتوى التطبيق والبطاقات */
        .dashboard-container { position: relative; z-index: 10; padding: 20px; padding-top: 40px; padding-bottom: 90px; }
        .welcome-header { text-align: center; margin-bottom: 30px; }
        .welcome-header h2 { font-size: 2rem; color: var(--accent); margin-bottom: 5px; }
        .welcome-header p { color: var(--light); font-size: 1.1rem; opacity: 0.9; }

        .cards-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 15px; max-width: 600px; margin: 0 auto; }
        .action-card { 
            background: rgba(255, 255, 255, 0.1); backdrop-filter: blur(10px); 
            border: 2px solid rgba(255,255,255,0.2); border-radius: 20px; 
            padding: 20px; text-align: center; cursor: pointer; 
            transition: transform 0.3s, background 0.3s; 
            display: flex; flex-direction: column; align-items: center; justify-content: center; min-height: 140px; 
        }
        .action-card:active { transform: scale(0.95); background: rgba(255, 255, 255, 0.2); }
        .action-card .icon { font-size: 3rem; margin-bottom: 10px; animation: float 4s ease-in-out infinite; }
        .action-card h3 { font-size: 1.1rem; color: var(--light); font-weight: 600; }
        
        /* تأخير حركة البطاقات لتبدو أجمل */
        .card-quran .icon { animation-delay: 0s; }
        .card-arabic .icon { animation-delay: 0.5s; }
        .card-islamic .icon { animation-delay: 1s; }
        .card-stories .icon { animation-delay: 1.5s; }
        .card-quiz .icon { animation-delay: 2s; }
        .card-games .icon { animation-delay: 2.5s; }

        /* القائمة السفلية */
        .bottom-nav { 
            position: fixed; bottom: 0; left: 0; width: 100%; 
            background: var(--primary-dark); display: flex; justify-content: space-around; 
            padding: 12px 0; border-top-left-radius: 25px; border-top-right-radius: 25px; 
            z-index: 100; box-shadow: 0 -5px 15px rgba(0,0,0,0.3); 
        }
        .nav-item { 
            color: var(--light); text-decoration: none; display: flex; 
            flex-direction: column; align-items: center; font-size: 0.8rem; 
            opacity: 0.6; transition: all 0.3s; 
        }
        .nav-item.active { opacity: 1; color: var(--accent); transform: translateY(-5px); }
        .nav-icon { font-size: 1.6rem; margin-bottom: 4px; }

        /* صفحات إضافية */
        .page-content { text-align: center; padding-top: 50px; }
        .page-icon { font-size: 5rem; margin-bottom: 20px; animation: float 3s infinite; }
        .btn-primary { 
            margin-top: 20px; padding: 15px 30px; background: var(--primary); 
            border: none; border-radius: 25px; color: white; font-size: 1.2rem; 
            font-weight: bold; cursor: pointer; box-shadow: 0 4px 10px rgba(0,0,0,0.3);
        }
    </style>
</head>
<body>

    <!-- شاشة البداية المدمجة -->
    <div id="splash-screen" class="splash-screen">
        <div class="logo-container">
            <svg class="splash-logo" viewBox="0 0 512 512" width="120" height="120">
                <rect width="512" height="512" rx="115" fill="#2EC4B6"/>
                <path d="M256,150 C170,230 160,330 160,350 L352,350 C352,330 342,230 256,150 Z" fill="#FFD166"/>
                <rect x="140" y="350" width="232" height="50" rx="12" fill="#FDFFFC" />
                <path d="M256,70 A35,35 0 1,0 291,105 A25,25 0 1,1 256,70 Z" fill="#FDFFFC"/>
            </svg>
            <h1 class="splash-title">Islam Kids</h1>
        </div>
    </div>
    
    <!-- حاوية التطبيق الرئيسية -->
    <div id="app" class="hidden">
        <!-- الخلفية المتحركة -->
        <div class="sky-bg">
            <div class="stars"></div>
            <div class="clouds">
                <div class="cloud" style="top: 20px; animation-duration: 25s;"></div>
                <div class="cloud" style="top: 90px; left: -50px; transform: scale(0.7); animation-duration: 35s;"></div>
            </div>
            <div class="mosque-bg"></div>
        </div>

        <!-- منطقة عرض الصفحات -->
        <main id="router-view"></main>

        <!-- القائمة السفلية المدمجة -->
        <nav id="bottomnav">
            <div class="bottom-nav">
                <a href="#/" class="nav-item active" id="nav-home">
                    <span class="nav-icon">🏠</span>
                    <span>الرئيسية</span>
                </a>
                <a href="#/quran" class="nav-item" id="nav-quran">
                    <span class="nav-icon">📖</span>
                    <span>القرآن</span>
                </a>
                <a href="#/profile" class="nav-item" id="nav-profile">
                    <span class="nav-icon">👤</span>
                    <span>حسابي</span>
                </a>
            </div>
        </nav>
    </div>

    <!-- =========================================
         JavaScript: برمجة التطبيق والتنقل
         ========================================= -->
    <script>
        // إخفاء شاشة البداية بعد ثانيتين
        document.addEventListener('DOMContentLoaded', () => {
            setTimeout(() => {
                const splash = document.getElementById('splash-screen');
                const app = document.getElementById('app');
                splash.classList.add('fade-out');
                setTimeout(() => {
                    splash.style.display = 'none';
                    app.classList.remove('hidden');
                    app.classList.add('fade-in');
                    handleRoute(); // تشغيل الصفحة الأولى
                }, 500);
            }, 2000);
        });

        // نظام التنقل بين الصفحات (Router)
        window.addEventListener('hashchange', handleRoute);

        function handleRoute() {
            const path = window.location.hash || '#/';
            const routerView = document.getElementById('router-view');
            
            // تحديث الأزرار في القائمة السفلية
            document.querySelectorAll('.nav-item').forEach(el => el.classList.remove('active'));
            if(path === '#/') document.getElementById('nav-home').classList.add('active');
            if(path === '#/quran') document.getElementById('nav-quran').classList.add('active');
            if(path === '#/profile') document.getElementById('nav-profile').classList.add('active');

            // عرض محتوى الصفحة بناءً على الرابط
            if (path === '#/') {
                routerView.innerHTML = `
                    <div class="dashboard-container fade-in">
                        <div class="welcome-header">
                            <h2>السلام عليكم</h2>
                            <p>مرحباً بك في أطفال الإسلام</p>
                        </div>
                        <div class="cards-grid">
                            <div class="action-card card-quran" onclick="window.location.hash='#/quran'">
                                <div class="icon">📖</div>
                                <h3>القرآن الكريم</h3>
                            </div>
                            <div class="action-card card-arabic" onclick="alert('قريباً إن شاء الله!')">
                                <div class="icon">🔤</div>
                                <h3>الحروف العربية</h3>
                            </div>
                            <div class="action-card card-islamic" onclick="alert('قريباً إن شاء الله!')">
                                <div class="icon">🕌</div>
                                <h3>تربية إسلامية</h3>
                            </div>
                            <div class="action-card card-stories" onclick="alert('قريباً إن شاء الله!')">
                                <div class="icon">📚</div>
                                <h3>قصص الأنبياء</h3>
                            </div>
                            <div class="action-card card-quiz" onclick="alert('قريباً إن شاء الله!')">
                                <div class="icon">❓</div>
                                <h3>اختبر معلوماتك</h3>
                            </div>
                            <div class="action-card card-games" onclick="alert('قريباً إن شاء الله!')">
                                <div class="icon">🎮</div>
                                <h3>ألعاب مفيدة</h3>
                            </div>
                        </div>
                    </div>
                `;
            } else if (path === '#/quran') {
                routerView.innerHTML = `
                    <div class="dashboard-container fade-in">
                        <div class="welcome-header">
                            <h2>القرآن الكريم</h2>
                            <p>استمع وتعلم كلام الله</p>
                        </div>
                        <div class="page-content">
                            <div class="page-icon">📖</div>
                            <h3 style="color:var(--light); font-size: 1.8rem;">سورة الفاتحة</h3>
                            <button class="btn-primary" onclick="alert('سيتم تشغيل الصوت هنا')">▶ تشغيل الصوت</button>
                        </div>
                    </div>
                `;
            } else if (path === '#/profile') {
                routerView.innerHTML = `
                    <div class="dashboard-container fade-in">
                        <div class="welcome-header">
                            <h2>حسابي الشخصي</h2>
                        </div>
                        <div class="page-content">
                            <div style="width: 120px; height: 120px; background: var(--accent); border-radius: 50%; margin: 0 auto; display:flex; align-items:center; justify-content:center; font-size:4rem; border: 4px solid white;">👦</div>
                            <h3 style="color:var(--light); margin-top:20px; font-size: 1.8rem;">المسلم الصغير</h3>
                            <p style="color: var(--secondary); margin-top: 5px; font-size: 1.2rem;">المستوى 5: بطل الإسلام</p>
                        </div>
                    </div>
                `;
            }
        }
    </script>
</body>
</html>
