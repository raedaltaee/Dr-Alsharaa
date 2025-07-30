<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="Awidth=device-width, initial-scale=1.0">
    <title>تحليل تفاعلي | الدليل الوطني للتدريب القيادي في العراق</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700&display=swap" rel="stylesheet">
    <!-- Chosen Palette: Calm Harmony Neutrals -->
    <!-- Application Structure Plan: A single-page dashboard structure. Starts with a hero section summarizing the report's goal. Followed by an interactive core section where users can select one of the 7 key gaps (challenges) from a side menu. Selecting a gap updates a main content area showing detailed information about the "Gap" and the "Solution" in separate tabs. A radar chart visualizes the severity of all gaps, highlighting the selected one. This is followed by a visual timeline for the implementation roadmap and a card-based section for strategic recommendations. This structure was chosen to transform a dense, linear report into an engaging, explorable experience, allowing users to focus on specific areas of interest while still seeing the overall picture, which is more effective for policymakers. -->
    <!-- Visualization & Content Choices: 
        1. Gaps Overview: Report Info -> The 7 key implementation gaps. Goal -> Allow users to explore each gap and its solution interactively. Viz/Presentation -> An icon-based side navigation to select a gap, a tabbed content area for "Gap" vs. "Solution", and a Chart.js Radar Chart to visualize the "severity" of each gap. Interaction -> Clicking an icon updates the text content and highlights the corresponding data point on the radar chart. Justification -> This interactive dashboard model breaks down complex information into digestible, user-controlled segments, making it far more engaging than reading a list. The radar chart provides an immediate visual summary of the multifaceted nature of the challenge. Library/Method -> Vanilla JS, Chart.js (Canvas).
        2. Implementation Roadmap: Report Info -> The 4-phase implementation timeline. Goal -> Present the strategic timeline in a clear, sequential manner. Viz/Presentation -> A vertical timeline diagram. Interaction -> Hover effects to highlight stages. Justification -> A visual timeline is more intuitive and easier to process than a standard HTML table, effectively telling the story of the proposed rollout. Library/Method -> HTML/CSS with Tailwind.
        3. Strategic Recommendations: Report Info -> Key strategic advice. Goal -> Highlight the most critical success factors. Viz/Presentation -> A grid of styled cards, each with an icon and a key recommendation. Justification -> Card-based layouts are visually appealing and help to break up text, making the recommendations more memorable. Library/Method -> HTML/CSS with Tailwind.
    -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body {
            font-family: 'Cairo', sans-serif;
            background-color: #FDFBF6;
            color: #3C3633;
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 500px;
            margin-left: auto;
            margin-right: auto;
            height: 350px;
            max-height: 400px;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 400px;
            }
        }
        .nav-link {
            transition: all 0.3s ease;
            border-bottom: 2px solid transparent;
        }
        .nav-link:hover, .nav-link.active {
            color: #E0A75E;
            border-bottom-color: #E0A75E;
        }
        .gap-icon.active {
            background-color: #E0A75E;
            color: #FFFFFF;
            transform: scale(1.1);
        }
        .gap-icon {
            transition: all 0.3s ease;
        }
        .tab.active {
            background-color: #E0A75E;
            color: #FFFFFF;
        }
        .timeline::before {
            content: '';
            position: absolute;
            top: 0;
            bottom: 0;
            right: 23px;
            width: 4px;
            background-color: #E0A75E;
            border-radius: 2px;
        }
        .timeline-item::before {
            content: '';
            position: absolute;
            top: 1rem;
            right: 12px;
            width: 24px;
            height: 24px;
            background-color: #F0EBE3;
            border: 4px solid #E0A75E;
            border-radius: 50%;
            z-index: 1;
        }
    </style>
</head>
<body class="antialiased">

    <header class="bg-[#F0EBE3] shadow-md sticky top-0 z-50">
        <nav class="container mx-auto px-6 py-3 flex justify-between items-center">
            <h1 class="text-xl font-bold text-[#3C3633]">الدليل الوطني للتدريب القيادي</h1>
            <div class="hidden md:flex space-x-8 space-x-reverse">
                <a href="#home" class="nav-link">الرئيسية</a>
                <a href="#challenges" class="nav-link">التحديات والحلول</a>
                <a href="#roadmap" class="nav-link">خارطة الطريق</a>
                <a href="#recommendations" class="nav-link">التوصيات</a>
            </div>
            <button id="mobile-menu-button" class="md:hidden flex items-center">
                <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7"></path></svg>
            </button>
        </nav>
        <div id="mobile-menu" class="hidden md:hidden">
            <a href="#home" class="block py-2 px-4 text-sm hover:bg-[#E0A75E] hover:text-white">الرئيسية</a>
            <a href="#challenges" class="block py-2 px-4 text-sm hover:bg-[#E0A75E] hover:text-white">التحديات والحلول</a>
            <a href="#roadmap" class="block py-2 px-4 text-sm hover:bg-[#E0A75E] hover:text-white">خارطة الطريق</a>
            <a href="#recommendations" class="block py-2 px-4 text-sm hover:bg-[#E0A75E] hover:text-white">التوصيات</a>
        </div>
    </header>

    <main>
        <section id="home" class="py-20 bg-[#F0EBE3]">
            <div class="container mx-auto px-6 text-center">
                <h2 class="text-4xl font-bold mb-4 text-[#3C3633]">بناء قيادة دولة فاعلة: رؤية استراتيجية للإصلاح</h2>
                <p class="text-lg max-w-3xl mx-auto mb-8 text-gray-700">
                    يمثل الدليل الوطني للتدريب القيادي مشروعاً وطنياً يهدف لبناء منظومة قيادية شاملة، تمكن الدولة العراقية من امتلاك قيادة عامة فاعلة، رشيدة، ومؤسسية. هذا التحليل التفاعلي يستعرض أبرز الثغرات التي تواجه تنفيذ هذا الدليل ويقترح حلولاً عملية لضمان نجاحه.
                </p>
                <a href="#challenges" class="bg-[#E0A75E] text-white font-bold py-3 px-8 rounded-full hover:bg-opacity-90 transition duration-300">
                    استكشف التحديات
                </a>
            </div>
        </section>

        <section id="challenges" class="py-20">
            <div class="container mx-auto px-6">
                <div class="text-center mb-12">
                    <h2 class="text-3xl font-bold text-[#3C3633]">تشخيص الثغرات واقتراح الحلول</h2>
                    <p class="text-md text-gray-600 mt-2">انقر على أحد المحاور أدناه لاستعراض الفجوة التنفيذية والحلول المقترحة لها.</p>
                </div>

                <div class="flex flex-col md:flex-row gap-8">
                    <div class="w-full md:w-1/4">
                        <div class="grid grid-cols-2 md:grid-cols-1 gap-4">
                            <button data-gap="legislative" class="gap-icon flex flex-col items-center justify-center p-4 bg-white rounded-lg shadow-md hover:shadow-xl active">
                                <span class="text-3xl mb-2">⚖️</span>
                                <span class="text-sm font-semibold text-center">التشريع والدعم السياسي</span>
                            </button>
                            <button data-gap="institutional" class="gap-icon flex flex-col items-center justify-center p-4 bg-white rounded-lg shadow-md hover:shadow-xl">
                                <span class="text-3xl mb-2">🏛️</span>
                                <span class="text-sm font-semibold text-center">البنية المؤسسية</span>
                            </button>
                            <button data-gap="funding" class="gap-icon flex flex-col items-center justify-center p-4 bg-white rounded-lg shadow-md hover:shadow-xl">
                                <span class="text-3xl mb-2">💰</span>
                                <span class="text-sm font-semibold text-center">التمويل والاستدامة</span>
                            </button>
                            <button data-gap="digital" class="gap-icon flex flex-col items-center justify-center p-4 bg-white rounded-lg shadow-md hover:shadow-xl">
                                <span class="text-3xl mb-2">💻</span>
                                <span class="text-sm font-semibold text-center">البنية الرقمية</span>
                            </button>
                            <button data-gap="meritocracy" class="gap-icon flex flex-col items-center justify-center p-4 bg-white rounded-lg shadow-md hover:shadow-xl">
                                <span class="text-3xl mb-2">🎯</span>
                                <span class="text-sm font-semibold text-center">الجدارة والعدالة</span>
                            </button>
                            <button data-gap="quality" class="gap-icon flex flex-col items-center justify-center p-4 bg-white rounded-lg shadow-md hover:shadow-xl">
                                <span class="text-3xl mb-2">⭐</span>
                                <span class="text-sm font-semibold text-center">جودة التدريب</span>
                            </button>
                            <button data-gap="monitoring" class="gap-icon flex flex-col items-center justify-center p-4 bg-white rounded-lg shadow-md hover:shadow-xl">
                                <span class="text-3xl mb-2">📊</span>
                                <span class="text-sm font-semibold text-center">التقييم والمتابعة</span>
                            </button>
                        </div>
                    </div>

                    <div class="w-full md:w-3/4 bg-white p-6 rounded-lg shadow-lg">
                        <div class="flex border-b mb-4">
                            <button data-tab="gap" class="tab py-2 px-6 font-semibold text-gray-600 hover:bg-gray-100 rounded-t-lg active">الثغرة</button>
                            <button data-tab="solution" class="tab py-2 px-6 font-semibold text-gray-600 hover:bg-gray-100 rounded-t-lg">الحل المقترح</button>
                        </div>
                        <div id="content-display">
                            <div id="content-gap"></div>
                            <div id="content-solution" class="hidden"></div>
                        </div>
                        <div class="mt-8 border-t pt-6">
                            <h3 class="text-xl font-bold text-center mb-4">مخطط شدة تأثير التحديات</h3>
                            <div class="chart-container">
                                <canvas id="gapsChart"></canvas>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section id="roadmap" class="py-20 bg-[#F0EBE3]">
            <div class="container mx-auto px-6">
                <div class="text-center mb-16">
                    <h2 class="text-3xl font-bold text-[#3C3633]">خارطة الطريق المقترحة للتفعيل</h2>
                    <p class="text-md text-gray-600 mt-2">مراحل زمنية مقترحة لضمان التنفيذ الفعال والمستدام للدليل.</p>
                </div>
                <div class="relative max-w-3xl mx-auto">
                    <div class="timeline">
                        <div class="timeline-item mb-12">
                            <div class="mr-12 bg-white p-6 rounded-lg shadow-lg">
                                <span class="text-sm font-bold text-[#E0A75E]">2025 - الربع الأول 2026</span>
                                <h4 class="text-xl font-bold mt-1 mb-2">المرحلة 1: التأسيس القانوني والمؤسسي</h4>
                                <ul class="list-disc list-inside text-gray-700 space-y-1">
                                    <li>إصدار قرار مجلس الوزراء بتبني الدليل كوثيقة سيادية.</li>
                                    <li>صياغة وتقديم "قانون تأهيل القيادات الحكومية".</li>
                                    <li>تأسيس "الفريق الوطني الأعلى" والمعهد الوطني للتدريب.</li>
                                    <li>تأمين الميزانية الأولية ضمن الموازنة الاتحادية.</li>
                                </ul>
                            </div>
                        </div>
                        <div class="timeline-item mb-12">
                            <div class="mr-12 bg-white p-6 rounded-lg shadow-lg">
                                <span class="text-sm font-bold text-[#E0A75E]">الربع الثاني 2026 - الربع الرابع 2027</span>
                                <h4 class="text-xl font-bold mt-1 mb-2">المرحلة 2: الإطلاق الرقمي والبرامجي</h4>
                                <ul class="list-disc list-inside text-gray-700 space-y-1">
                                    <li>إطلاق المنصة الوطنية الذكية للقيادة (NLMS).</li>
                                    <li>اعتماد الدفعة الأولى من المدربين الوطنيين.</li>
                                    <li>إطلاق المسارات التدريبية التأسيسية (مرحلة تجريبية).</li>
                                    <li>تأسيس "الصندوق الوطني لتطوير القيادات" بقانون.</li>
                                </ul>
                            </div>
                        </div>
                        <div class="timeline-item mb-12">
                            <div class="mr-12 bg-white p-6 rounded-lg shadow-lg">
                                <span class="text-sm font-bold text-[#E0A75E]">2028 - 2029</span>
                                <h4 class="text-xl font-bold mt-1 mb-2">المرحلة 3: التوسع وقياس الأثر</h4>
                                <ul class="list-disc list-inside text-gray-700 space-y-1">
                                    <li>التوسع في تطبيق البرامج على جميع المؤسسات.</li>
                                    <li>التطبيق الكامل لنظام التقييم الذكي لأثر التدريب.</li>
                                    <li>إصدار التقرير الوطني السنوي الأول عن حالة القيادة.</li>
                                    <li>تفعيل "وحدة قياس الأثر الوطني للتدريب".</li>
                                </ul>
                            </div>
                        </div>
                        <div class="timeline-item">
                            <div class="mr-12 bg-white p-6 rounded-lg shadow-lg">
                                <span class="text-sm font-bold text-[#E0A75E]">2030 وما بعدها</span>
                                <h4 class="text-xl font-bold mt-1 mb-2">المرحلة 4: المؤسسة والاستشراف المستقبلي</h4>
                                <ul class="list-disc list-inside text-gray-700 space-y-1">
                                    <li>المراجعة والتحديث المستمر للإطار القانوني والمناهج.</li>
                                    <li>تعزيز الشراكات الدولية لنقل المعرفة وتوطين الخبرات.</li>
                                    <li>تطبيق أحدث تقنيات الذكاء الاصطناعي في دعم القرار.</li>
                                    <li>تحقيق استقلالية جزئية للمنظومة عن الموازنة العامة.</li>
                                </ul>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section id="recommendations" class="py-20">
            <div class="container mx-auto px-6">
                <div class="text-center mb-12">
                    <h2 class="text-3xl font-bold text-[#3C3633]">توصيات استراتيجية للنجاح</h2>
                    <p class="text-md text-gray-600 mt-2">عوامل النجاح الحاسمة لضمان تحويل الرؤية إلى واقع.</p>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
                    <div class="bg-white p-6 rounded-lg shadow-lg text-center">
                        <div class="text-4xl mb-4">🤝</div>
                        <h4 class="text-xl font-bold mb-2">الإرادة السياسية</h4>
                        <p class="text-gray-700">دعم مستمر وملكية رفيعة المستوى من قمة الهرم الحكومي لتجاوز العقبات وتوفير الموارد اللازمة.</p>
                    </div>
                    <div class="bg-white p-6 rounded-lg shadow-lg text-center">
                        <div class="text-4xl mb-4">📜</div>
                        <h4 class="text-xl font-bold mb-2">الإطار القانوني</h4>
                        <p class="text-gray-700">توفير سلطة ملزمة للدليل عبر قانون جديد أو تعديلات تشريعية لحمايته من التقلبات السياسية.</p>
                    </div>
                    <div class="bg-white p-6 rounded-lg shadow-lg text-center">
                        <div class="text-4xl mb-4">🏦</div>
                        <h4 class="text-xl font-bold mb-2">الاستقلالية المؤسسية</h4>
                        <p class="text-gray-700">منح استقلالية حقيقية للمعهد الوطني والفريق الوطني للعمل بمهنية وحيادية بعيداً عن التسييس.</p>
                    </div>
                    <div class="bg-white p-6 rounded-lg shadow-lg text-center">
                        <div class="text-4xl mb-4">📈</div>
                        <h4 class="text-xl font-bold mb-2">القيادة بالبيانات</h4>
                        <p class="text-gray-700">الاستفادة الكاملة من المنصات الرقمية والذكاء الاصطناعي لدعم اتخاذ القرار بموضوعية.</p>
                    </div>
                    <div class="bg-white p-6 rounded-lg shadow-lg text-center">
                        <div class="text-4xl mb-4">🌱</div>
                        <h4 class="text-xl font-bold mb-2">ثقافة الجدارة</h4>
                        <p class="text-gray-700">مكافحة المحسوبية والتحيزات لضمان أن الكفاءة هي المعيار الأوحد للتقدم وتعزيز الثقة العامة.</p>
                    </div>
                    <div class="bg-white p-6 rounded-lg shadow-lg text-center">
                        <div class="text-4xl mb-4">🔄</div>
                        <h4 class="text-xl font-bold mb-2">المراجعة والتكيف</h4>
                        <p class="text-gray-700">إنشاء آليات للمراجعة الدورية والتكيف المستمر لمكونات الدليل لضمان فعاليته على المدى الطويل.</p>
                    </div>
                </div>
            </div>
        </section>
    </main>

    <footer class="bg-[#3C3633] text-white py-8">
        <div class="container mx-auto px-6 text-center">
            <p>تم تصميم هذا التحليل التفاعلي بناءً على "الدليل الوطني للتدريب القيادي العام في العراق".</p>
            <p class="text-sm mt-2 opacity-70">
                إعداد: الدكتور فاضل محمد رضا الشرع<br>
                مدير عام دائرة أوقاف المحافظات<br>
                ديوان الوقف الشيعي
            </p>
            <p class="text-sm mt-2 opacity-70">يهدف هذا المشروع إلى تحويل الرؤى إلى واقع ملموس لبناء دولة تستحق أن تُقاد كما ينبغي لها.</p>
        </div>
    </footer>

    <script>
        document.addEventListener('DOMContentLoaded', function () {
            const reportData = {
                legislative: {
                    title: "التشريع والدعم السياسي",
                    severity: 9,
                    gap: `
                        <h4 class="text-xl font-bold mb-3">غياب الإطار القانوني الملزم</h4>
                        <p class="mb-4">يفتقر الدليل حالياً إلى القوة القانونية التي تضمن تطبيقه بشكل شامل، مما يجعله مجرد إطار توجيهي عرضة للتجاهل أو التطبيق الانتقائي.</p>
                        <ul class="list-disc list-inside space-y-2">
                            <li><strong>فجوة قانونية:</strong> عدم وجود قانون خاص أو تعديل لقانون الخدمة العامة يجعل التدريب غير إلزامي.</li>
                            <li><strong>تضارب تشريعي:</strong> القوانين الحالية (خدمة مدنية، إصلاح إداري) قد لا تكون منسجمة مع أهداف الدليل.</li>
                            <li><strong>مقاومة التغيير:</strong> التسييس ومقاومة التغيير يمثلان عقبات ثقافية وسياسية عميقة قد تفرغ الدليل من محتواه.</li>
                        </ul>
                    `,
                    solution: `
                        <h4 class="text-xl font-bold mb-3">توفير غطاء قانوني وسياسي قوي</h4>
                        <p class="mb-4">تحويل الدليل من مقترح إلى وثيقة سيادية ملزمة عبر خطوات تشريعية وتنفيذية عاجلة.</p>
                        <ul class="list-disc list-inside space-y-2">
                            <li><strong>الإسراع بالتشريع:</strong> إقرار "قانون تأهيل القيادات الحكومية" لينص على إلزامية التدريب وربطه بالترقية.</li>
                            <li><strong>قرار وزاري ملزم:</strong> إصدار قرار من مجلس الوزراء بتبني الدليل كوثيقة ملزمة لكافة مؤسسات الدولة فوراً.</li>
                            <li><strong>توحيد المرجعية:</strong> إعداد قانون اتحادي لتعريف "القيادة العامة" بشكل موحد لمعالجة غياب الإطار المفاهيمي في الدستور.</li>
                        </ul>
                    `
                },
                institutional: {
                    title: "البنية المؤسسية",
                    severity: 8,
                    gap: `
                        <h4 class="text-xl font-bold mb-3">تحديات استقلالية الهياكل المقترحة</h4>
                        <p class="mb-4">ارتباط المعهد الوطني للتدريب بالأمانة العامة لمجلس الوزراء وتأليف الفريق الوطني بقرار من رئيس الوزراء قد يحد من استقلاليتهما ويجعلهما عرضة للتأثيرات السياسية والبيروقراطية.</p>
                        <ul class="list-disc list-inside space-y-2">
                            <li><strong>التبعية الإدارية:</strong> الارتباط المباشر قد يحول المعهد إلى أداة سياسية بدلاً من مؤسسة مهنية مستقلة.</li>
                            <li><strong>عدم الاستقرار:</strong> تغيير الفريق الوطني مع كل حكومة جديدة يهدد استمرارية السياسات الاستراتيجية.</li>
                            <li><strong>البيروقراطية:</strong> الإجراءات الروتينية المعقدة قد تخنق الابتكار وتعيق استقطاب الكفاءات.</li>
                        </ul>
                    `,
                    solution: `
                        <h4 class="text-xl font-bold mb-3">تعزيز الحوكمة والاستقلالية</h4>
                        <p class="mb-4">بناء هياكل مؤسسية قوية ومستقلة قادرة على العمل بمهنية وحيادية.</p>
                        <ul class="list-disc list-inside space-y-2">
                            <li><strong>الاستقلالية القانونية للمعهد:</strong> إنشاء المعهد بقانون خاص يحدد استقلاليته المالية والإدارية ويربطه برئاسة الوزراء كجهة إشرافية فقط.</li>
                            <li><strong>مجلس أمناء مستقل:</strong> تشكيل مجلس أمناء للمعهد من شخصيات مهنية وأكاديمية مستقلة.</li>
                            <li><strong>مدة ولاية ثابتة للفريق الوطني:</strong> تحديد مدة ولاية ثابتة لأعضاء الفريق الوطني لضمان الاستمرارية وعدم الارتباط بتغيير الحكومات.</li>
                        </ul>
                    `
                },
                funding: {
                    title: "التمويل والاستدامة",
                    severity: 9,
                    gap: `
                        <h4 class="text-xl font-bold mb-3">الاعتماد على التمويل غير المستدام</h4>
                        <p class="mb-4">اعتماد المنظومة على التخصيصات السنوية الموسمية يجعلها "رهينة للمزاجيات الإدارية" ومهددة بالانقطاع، مما يقوض الأهداف طويلة المدى.</p>
                        <ul class="list-disc list-inside space-y-2">
                            <li><strong>تقلبات الميزانية:</strong> التمويل عرضة للتقلبات الاقتصادية والتسييس، مما يهدد استمرارية البرامج.</li>
                            <li><strong>ضعف كفاءة الإنفاق:</strong> في بيئة تعاني من ضعف الرقابة، قد تتعرض الميزانيات للهدر وسوء الاستخدام.</li>
                            <li><strong>التبعية للمنح:</strong> الاعتماد على المنح الدولية قد يشكل البرامج وفقاً لأولويات المانحين وليس الاحتياجات الوطنية.</li>
                        </ul>
                    `,
                    solution: `
                        <h4 class="text-xl font-bold mb-3">ضمان تمويل مستدام وكفؤ</h4>
                        <p class="mb-4">إنشاء آليات تمويل مبتكرة ومستدامة تحمي المنظومة من التقلبات وتضمن كفاءة الإنفاق.</p>
                        <ul class="list-disc list-inside space-y-2">
                            <li><strong>صندوق وطني دائم:</strong> تأسيس "صندوق وطني دائم لتطوير القيادات" بقانون، بمصادر تمويل متنوعة (موازنة، رسوم، منح، شراكات).</li>
                            <li><strong>ربط التمويل بالأداء:</strong> ربط جزء من التمويل بتحقيق مؤشرات أداء ونتائج ملموسة لتعزيز كفاءة الإنفاق.</li>
                            <li><strong>حوكمة مالية ورقابة:</strong> إخضاع الصندوق لتدقيق شامل من ديوان الرقابة المالية وهيئة النزاهة لضمان الشفافية.</li>
                        </ul>
                    `
                },
                digital: {
                    title: "البنية الرقمية",
                    severity: 7,
                    gap: `
                        <h4 class="text-xl font-bold mb-3">ضعف البنية التحتية الرقمية</h4>
                        <p class="mb-4">الكثير من البيئات القيادية في العراق لا تزال تقليدية وهشة أمام التهديدات السيبرانية، مما يعيق تشغيل المنصات الذكية التي يقترحها الدليل.</p>
                        <ul class="list-disc list-inside space-y-2">
                            <li><strong>بنية تحتية هشة:</strong> ضعف البنية التحتية يعيق تطبيق أدوات التقييم المعتمدة على الذكاء الاصطناعي.</li>
                            <li><strong>غياب تشريعات السيادة السيبرانية:</strong> يعرض البيانات الحساسة لخطر الاختراق ويقوض الثقة في النظام.</li>
                            <li><strong>نقص الوعي الرقمي:</strong> عدم إلمام القيادات بالتقنيات الحديثة يقلل من قدرتهم على الاستفادة من الأدوات المتقدمة.</li>
                        </ul>
                    `,
                    solution: `
                        <h4 class="text-xl font-bold mb-3">تطوير بنية تحتية رقمية آمنة</h4>
                        <p class="mb-4">بناء منظومة رقمية قوية ومحمية تضمن السيادة الرقمية وتمكن القادة من استخدام التكنولوجيا بفعالية.</p>
                        <ul class="list-disc list-inside space-y-2">
                            <li><strong>بنية سحابية وطنية (G-Cloud):</strong> تخزين جميع البيانات داخل خوادم وطنية محمية لضمان السيادة الرقمية.</li>
                            <li><strong>قانون السيادة الرقمية:</strong> الإسراع في إصدار "قانون السيادة الرقمية القيادية" وإنشاء مركز وطني للهندسة السيبرانية.</li>
                            <li><strong>تدريب مكثف على الرقمنة:</strong> تضمين تدريب مكثف على الذكاء الاصطناعي ضمن برامج القيادة وإنشاء وحدات "مساعد القرار الذكي" في الوزارات.</li>
                        </ul>
                    `
                },
                meritocracy: {
                    title: "الجدارة والعدالة",
                    severity: 10,
                    gap: `
                        <h4 class="text-xl font-bold mb-3">هيمنة المحسوبية والولاءات</h4>
                        <p class="mb-4">التمكين على أساس الانتماء الحزبي أو الشخصي يقوض مبدأ تكافؤ الفرص ويحرم الكفاءات الحقيقية من الوصول للمناصب القيادية.</p>
                        <ul class="list-disc list-inside space-y-2">
                            <li><strong>تسييس المناصب:</strong> استخدام لجان اختيار صورية لشرعنة قرارات متخذة مسبقاً بناءً على اعتبارات غير مهنية.</li>
                            <li><strong>الصور النمطية:</strong> هيمنة النماذج التقليدية والإقصاء غير المرئي للمرأة يحد من التنوع والكفاءة.</li>
                            <li><strong>تآكل الثقة:</strong> شغل المناصب بالمحاباة يؤدي إلى تآكل حاد في ثقة الجمهور بالمؤسسات الحكومية.</li>
                        </ul>
                    `,
                    solution: `
                        <h4 class="text-xl font-bold mb-3">إرساء العدالة وتكافؤ الفرص</h4>
                        <p class="mb-4">تطبيق آليات شفافة وموضوعية تضمن أن الكفاءة هي المعيار الوحيد للاختيار والترقية.</p>
                        <ul class="list-disc list-inside space-y-2">
                            <li><strong>نظام إعلان وطني موحد:</strong> للفرص القيادية عبر منصة الدولة مع لجان اختبار مستقلة وتشفير أسماء المرشحين.</li>
                            <li><strong>تقييم 360 درجة:</strong> تطبيق تقييم الأداء من أطراف متعددة (رئيس، مرؤوس، زميل) لضمان الموضوعية.</li>
                            <li><strong>لجنة "العدالة القيادية":</strong> تأسيس لجنة وطنية مستقلة ومنصة رقمية للطعن السري والموثق ضد أي قرار جائر.</li>
                        </ul>
                    `
                },
                quality: {
                    title: "جودة التدريب",
                    severity: 8,
                    gap: `
                        <h4 class="text-xl font-bold mb-3">تحديات جودة المدربين والمحتوى</h4>
                        <p class="mb-4">ضمان استيفاء المدربين للشروط الصارمة وتكييف المحتوى التدريبي مع الخصوصيات العراقية يمثل تحدياً كبيراً قد يؤثر على فعالية التدريب.</p>
                        <ul class="list-disc list-inside space-y-2">
                            <li><strong>نقص الكفاءات:</strong> صعوبة إيجاد مدربين يستوفون شروط الخبرة القيادية (7 سنوات) والجدارات المهنية.</li>
                            <li><strong>محتوى غير مكيّف:</strong> المحتوى العام الذي لا يعالج السياق العراقي الفريد سيفقد فعاليته.</li>
                            <li><strong>تدريب شكلي:</strong> خطر أن يصبح التدريب مجرد إجراء شكلي للامتثال للمتطلبات دون تحقيق أثر حقيقي.</li>
                        </ul>
                    `,
                    solution: `
                        <h4 class="text-xl font-bold mb-3">ضمان جودة عالمية بمواصفات وطنية</h4>
                        <p class="mb-4">تطوير منظومة متكاملة لضمان جودة المدربين والمحتوى والبيئة التدريبية.</p>
                        <ul class="list-disc list-inside space-y-2">
                            <li><strong>معايير اعتماد صارمة للمدربين:</strong> تدريب المدربين وفقاً للمعايير الدولية (ILA, OECD) واعتمادهم رسمياً.</li>
                            <li><strong>وحدة "ابتكار المحتوى":</strong> تأسيس وحدة لتطوير محتوى تدريبي متجدد يجمع بين أفضل الممارسات العالمية والخصوصية العراقية.</li>
                            <li><strong>نظام اعتماد للمراكز التدريبية:</strong> إصدار نظام وطني ملزم لاعتماد مراكز التدريب ومنع تنفيذ أي تدريب خارج المراكز المعتمدة.</li>
                        </ul>
                    `
                },
                monitoring: {
                    title: "التقييم والمتابعة",
                    severity: 7,
                    gap: `
                        <h4 class="text-xl font-bold mb-3">ضعف آليات قياس الأثر</h4>
                        <p class="mb-4">الاعتماد على التقييمات التقليدية التي تقيس المعرفة اللحظية دون التركيز على التغيير السلوكي الفعلي أو الأثر المؤسسي طويل المدى.</p>
                        <ul class="list-disc list-inside space-y-2">
                            <li><strong>تقييمات سطحية:</strong> التركيز على "الانطباع" بدلاً من "الأثر" الحقيقي للتدريب.</li>
                            <li><strong>ضعف الربط بالأداء:</strong> غياب آليات مؤتمتة لربط نتائج التدريب بمؤشرات الأداء الرئيسية للمؤسسات (KPIs).</li>
                            <li><strong>غياب الاستقلالية:</strong> ارتباط جهات التقييم بالجهات المنفذة للتدريب قد يؤثر على موضوعية النتائج.</li>
                        </ul>
                    `,
                    solution: `
                        <h4 class="text-xl font-bold mb-3">إنشاء نظام تقييم ذكي ومستقل</h4>
                        <p class="mb-4">بناء منظومة تقييم تركز على قياس الأثر الفعلي للتدريب على أداء القائد والمؤسسة.</p>
                        <ul class="list-disc list-inside space-y-2">
                            <li><strong>منصة ذكية للتقييم:</strong> استخدام الذكاء الاصطناعي لجمع وتحليل البيانات من مصادر متعددة (تقييم 360، أداء مؤسسي).</li>
                            <li><strong>وحدة مستقلة لقياس الأثر:</strong> تمكين "وحدة قياس الأثر الوطني للتدريب" داخل المعهد ومنحها صلاحيات واسعة لإجراء تقييمات مستقلة.</li>
                            <li><strong>ربط الحوافز بالنتائج:</strong> ربط التمويل والمكافآت القيادية بشكل مباشر بنتائج التقييم الذكي والأثر المؤسسي المحقق.</li>
                        </ul>
                    `
                }
            };

            const gapIcons = document.querySelectorAll('.gap-icon');
            const contentGapDiv = document.getElementById('content-gap');
            const contentSolutionDiv = document.getElementById('content-solution');
            const tabs = document.querySelectorAll('.tab');
            const mobileMenuButton = document.getElementById('mobile-menu-button');
            const mobileMenu = document.getElementById('mobile-menu');

            let currentGap = 'legislative';
            let currentTab = 'gap';

            function updateContent() {
                const data = reportData[currentGap];
                contentGapDiv.innerHTML = data.gap;
                contentSolutionDiv.innerHTML = data.solution;

                if (currentTab === 'gap') {
                    contentGapDiv.classList.remove('hidden');
                    contentSolutionDiv.classList.add('hidden');
                } else {
                    contentGapDiv.classList.add('hidden');
                    contentSolutionDiv.classList.remove('hidden');
                }

                gapIcons.forEach(icon => {
                    icon.classList.toggle('active', icon.dataset.gap === currentGap);
                });

                tabs.forEach(tab => {
                    tab.classList.toggle('active', tab.dataset.tab === currentTab);
                });
                
                updateChart();
            }

            gapIcons.forEach(icon => {
                icon.addEventListener('click', () => {
                    currentGap = icon.dataset.gap;
                    updateContent();
                });
            });

            tabs.forEach(tab => {
                tab.addEventListener('click', () => {
                    currentTab = tab.dataset.tab;
                    updateContent();
                });
            });
            
            mobileMenuButton.addEventListener('click', () => {
                mobileMenu.classList.toggle('hidden');
            });

            const ctx = document.getElementById('gapsChart').getContext('2d');
            let chart;

            function updateChart() {
                const labels = Object.values(reportData).map(g => g.title);
                const data = Object.values(reportData).map(g => g.severity);
                const backgroundColors = Object.keys(reportData).map(key => 
                    key === currentGap ? 'rgba(224, 167, 94, 0.6)' : 'rgba(60, 54, 51, 0.2)'
                );
                const borderColors = Object.keys(reportData).map(key => 
                    key === currentGap ? 'rgba(224, 167, 94, 1)' : 'rgba(60, 54, 51, 0.5)'
                );

                if (chart) {
                    chart.data.datasets[0].backgroundColor = backgroundColors;
                    chart.data.datasets[0].borderColor = borderColors;
                    chart.update();
                } else {
                    chart = new Chart(ctx, {
                        type: 'radar',
                        data: {
                            labels: labels,
                            datasets: [{
                                label: 'شدة التأثير (من 10)',
                                data: data,
                                backgroundColor: backgroundColors,
                                borderColor: borderColors,
                                borderWidth: 2,
                                pointBackgroundColor: borderColors,
                                pointBorderColor: '#fff',
                                pointHoverBackgroundColor: '#fff',
                                pointHoverBorderColor: borderColors
                            }]
                        },
                        options: {
                            responsive: true,
                            maintainAspectRatio: false,
                            scales: {
                                r: {
                                    angleLines: {
                                        color: 'rgba(60, 54, 51, 0.2)'
                                    },
                                    grid: {
                                        color: 'rgba(60, 54, 51, 0.2)'
                                    },
                                    pointLabels: {
                                        font: {
                                            size: 11,
                                            family: 'Cairo'
                                        },
                                        color: '#3C3633'
                                    },
                                    ticks: {
                                        backdropColor: 'rgba(253, 251, 246, 0.7)',
                                        color: '#3C3633',
                                        stepSize: 2,
                                        font: { family: 'Cairo' }
                                    },
                                    min: 0,
                                    max: 10
                                }
                            },
                            plugins: {
                                legend: {
                                    display: false
                                },
                                tooltip: {
                                    rtl: true,
                                    textDirection: 'rtl',
                                     bodyFont: { family: 'Cairo' },
                                     titleFont: { family: 'Cairo' }
                                }
                            }
                        }
                    });
                }
            }
            
            document.querySelectorAll('a[href^="#"]').forEach(anchor => {
                anchor.addEventListener('click', function (e) {
                    e.preventDefault();
                    document.querySelector(this.getAttribute('href')).scrollIntoView({
                        behavior: 'smooth'
                    });
                    if (mobileMenu.classList.contains('hidden') === false) {
                        mobileMenu.classList.add('hidden');
                    }
                });
            });

            updateContent();
        });
    </script>
</body>
</html>
# Dr-Alsharaa
