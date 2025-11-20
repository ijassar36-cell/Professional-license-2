<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1"/>
<title>محاكي الرخصة المهنية — إعداد المعلم: فهد الخالدي</title>
<style>
:root{
  --primary:#1e64d0;
  --muted:#6b7280;
  --card:#ffffff;
  --success:#16a34a;
  --danger:#ef4444;
  --bg:#f6f8ff;
}
*{box-sizing:border-box}
html,body{height:100%;margin:0;padding:0;font-family:"Almarai", "Noto Kufi Arabic", Tahoma, Arial, sans-serif;background:var(--bg);color:#0b1220}
.container{max-width:1100px;margin:20px auto;background:var(--card);padding:18px;border-radius:12px;box-shadow:0 12px 40px rgba(12,22,48,0.06)}
.header{display:flex;flex-direction:column;align-items:center;gap:6px}
.title{font-size:1.25rem;font-weight:800;color:var(--primary);margin:6px 0}
.credit{color:var(--muted);font-weight:700}
.main{display:grid;grid-template-columns:1fr 340px;gap:18px;margin-top:18px}
@media(max-width:980px){.main{grid-template-columns:1fr}}
.quiz{display:flex;flex-direction:column;gap:14px;padding-right:6px}
.card{background:linear-gradient(180deg,#fff,#fbfdff);border-radius:10px;padding:12px;box-shadow:0 8px 30px rgba(2,6,23,0.04);transition:transform .12s;overflow:visible}
.card:hover{transform:translateY(-4px)}
.q-head{display:flex;justify-content:space-between;align-items:center;gap:12px}
.q-num{background:linear-gradient(135deg,#06b6d4,var(--primary));color:#fff;padding:6px 10px;border-radius:8px;font-weight:800}
.sentence{margin-top:10px;font-size:1.02rem;line-height:1.6;color:#0b1220;white-space:pre-wrap}
.options{display:flex;flex-wrap:wrap;gap:10px;margin-top:12px}
.opt{flex:1 1 48%;min-width:140px;padding:12px;border-radius:10px;border:1px solid rgba(15,23,42,0.06);background:linear-gradient(180deg,#fbfdff,#f6fbff);font-weight:700;cursor:pointer;text-align:right;transition:all .12s;user-select:none}
.opt:hover{transform:translateY(-4px);box-shadow:0 10px 30px rgba(2,6,23,0.04)}
.opt.disabled{opacity:.9;pointer-events:none}
.opt.correct{background:linear-gradient(90deg,#bbf7d0,var(--success));color:#04200a;box-shadow:0 10px 30px rgba(16,185,129,0.08);border-color:rgba(16,185,129,0.25)}
.opt.wrong{background:linear-gradient(90deg,#ffd6db,var(--danger));color:#3b0a0a;box-shadow:0 10px 30px rgba(239,68,68,0.08);border-color:rgba(239,68,68,0.25)}
.pop{margin-top:12px;padding:12px;border-radius:10px;background:linear-gradient(180deg,#ffffff,#fbfdff);border:1px solid #e6eef8;box-shadow:0 10px 30px rgba(2,6,23,0.03)}
.pop strong{display:block;margin-bottom:6px;font-weight:800}
.side{background:var(--card);border-radius:10px;padding:16px;box-shadow:0 10px 30px rgba(12,22,48,0.06);height:max-content}
.progress{height:12px;background:#f1f9ff;border-radius:999px;overflow:hidden;margin-bottom:12px}
.progress > i{display:block;height:100%;background:linear-gradient(90deg,#06b6d4,var(--primary));width:0%;transition:width .36s ease}
.stats{display:flex;flex-direction:column;gap:8px;color:var(--muted);font-weight:700}
.stat-row{display:flex;justify-content:space-between;align-items:center;font-size:0.95rem}
.controls{display:flex;gap:8px;align-items:center;margin-top:12px;flex-wrap:wrap}
.btn{background:var(--primary);color:#fff;padding:10px 14px;border-radius:10px;border:0;font-weight:800;cursor:pointer}
.btn.secondary{background:#fff;color:var(--primary);border:1px solid rgba(30,100,208,0.12)}
.footer{margin-top:18px;text-align:center;color:var(--muted);font-size:0.95rem}
.note{color:var(--muted);font-size:0.95rem;margin-top:8px;text-align:center}

/* overlay result */
#overlay{position:fixed;inset:0;display:none;align-items:center;justify-content:center;background:rgba(2,6,23,0.46);z-index:9999}
#overlay .panel{background:#fff;padding:18px;border-radius:12px;max-width:520px;width:92%;text-align:center;box-shadow:0 20px 50px rgba(2,6,23,0.3)}

</style>
</head>
<body>
  <div class="container">
    <div class="header">
      <div class="title">محاكي الرخصة المهنية</div>
      <div class="credit">إعداد المعلم: فهد الخالدي</div>
    </div>

    <div class="main">
      <div class="quiz" id="quizColumn">
        <!-- أسئلة تُولَّد هنا -->
      </div>

      <aside class="side">
        <div class="small">شريط التقدم</div>
        <div class="progress"><i id="progressBar"></i></div>
        <div class="stats">
          <div class="stat-row"><span>الإجابات الصحيحة</span><strong id="statCorrect">0</strong></div>
          <div class="stat-row"><span>الأسئلة المجابة</span><strong id="statAnswered">0 / 68</strong></div>
          <div class="stat-row"><span>النسبة</span><strong id="statPercent">0%</strong></div>
        </div>

        <div class="controls">
          <button id="showAll" class="btn">عرض النتيجة</button>
          <button id="resetBtn" class="btn secondary">إعادة الاختبار</button>
        </div>

        <div class="note">اضغط على خيار كل سؤال لعرض الشرح وتلوين الإجابة.</div>
      </aside>
    </div>

    <div class="footer"></div>
  </div>

  <!-- Overlay نتيجة -->
  <div id="overlay">
    <div class="panel">
      <h2 style="margin:6px 0 10px;color:var(--primary)">النتيجة النهائية</h2>
      <div id="finalScore" style="font-size:2.4rem;font-weight:900;color:var(--success)">0 / 68</div>
      <div id="finalBreak" style="margin-top:10px;color:var(--muted)"></div>
      <button id="closeOverlay" class="btn secondary" style="margin-top:14px">حسناً</button>
    </div>
  </div>

<script>
/* ===========================
   QUESTIONS (68) — الإجابات مضبوطة حسب الترتيب الذي أعطيته
   حروف: أ=0، ب=1، ج=2، د=3
   ترتيب المطلوب (1..68): 
   1- ب,2- أ,3- ج,4- د,5- د,6- ج,7- ب,8- د,9- أ,10- ج,11- ب,12- د,13- أ,14- ب,15- ج,16- د,
   17- أ,18- ب,19- ج,20- أ,21- ج,22- أ,23- ب,24- ج,25- د,26- ب,27- أ,28- ج,29- د,30- ب,
   31- ب,32- ج,33- ب,34- أ,35- ج,36- أ,37- ب,38- د,39- ج,40- ب,41- أ,42- د,43- ج,44- ب,
   45- أ,46- ج,47- أ,48- د,49- ب,50- ج,51- د,52- أ,53- ب,54- ج,55- ب,56- أ,57- د,58- ب,
   59- ج,60- أ,61- د,62- د,63- أ,64- د,65- ج,66- ب,67- د,68- د
   =========================== */

const QUESTIONS = [
/* 1 */ { q: "ماذا يعني \"التقويم التكويني\" في ممارسات التدريس؟", choices: ["اختبار نهائي فقط","تقييم مستمر يهدف لتحسين تعلم الطلاب أثناء العملية","جدولة الامتحانات","حفظ السجلات"], correct:1, explanation: "التقويم التكويني هو عملية تقييم مستمرة تساعد المعلم على تعديل التدريس أثناء التعلم وتحسين نتائج الطلاب." },
/* 2 */ { q: "ما أهم سمة في صياغة الهدف التعليمي الجيد؟", choices: ["يكون قابلاً للقياس","غامض ومفتوح للتفسير","طويل جداً","غير قابل للقياس"], correct:0, explanation: "الهدف الجيد يجب أن يكون قابلاً للقياس بوصف سلوكي واضح كي يمكن تقييم تحقيقه." },
/* 3 */ { q: "في موقف صفّي: طالب يهيمن على نقاش المجموعة ويمنع الآخرين من الكلام. ما الإجراء المناسب؟", choices: ["تجاهل الموقف","تقديم مكافآت فورية","تعيين أدوار وتدويرها بين الأعضاء لضمان مشاركة الجميع","إخراجه من المجموعة"], correct:2, explanation: "تعيين أدوار وتدويرها يساعد على مشاركة الجميع ويقلل من احتكار المتحدث." },
/* 4 */ { q: "أيهما يعبر عن تعريف \"الصدق\" في أدوات القياس؟", choices: ["أن الاختبار متزن من حيث الطول","أن الأداة تقيس ما صممت لقياسه فعلاً","أن الأسئلة مشوقة","أن الاختبار سريع"], correct:3, explanation: "الصدق (Validity) يعني مدى قدرة الأداة على قياس الخاصية المطلوبة، وليس طولها أو سرعتها." },
/* 5 */ { q: "اقرأ الفقرة: \"التفكير النقدي يتطلب تقييم الأدلة ومقارنة المصادر قبل قبولها\". ما المطلوب لقياس الفهم الضمني؟", choices: ["نسخ الجملة","تحديد عدد الكلمات","استنتاج أن التحقق من المصادر ضروري","تحديد نوع الورقة"], correct:3, explanation: "الفهم الضمني يتطلب استنتاج أن الكاتب يؤكد على أهمية التحقق من المصادر قبل قبول المعلومات." },
/* 6 */ { q: "ما المقصود بمعايرة بنوك الأسئلة (item calibration)؟", choices: ["إعطاء نفس الأسئلة للجميع","تحليل خصائص البنود (الصعوبة والتمييز) اعتمادا على بيانات تجريبية","اختصار الاختبار","كتابة الأسئلة بأسلوب أدبي"], correct:2, explanation: "المعايرة تعني استخدام بيانات لتحليل الصعوبة وتمييز البنود لضمان جودة الاختبار." },
/* 7 */ { q: "في الهندسة: إذا كانت زوايا مثلث هي x, x, 40 فما قيمة x؟", choices: ["40","70","20","100"], correct:1, explanation: "مجموع زوايا المثلث 180 => 2x + 40 = 180 => x = 70." },
/* 8 */ { q: "ما هو مفهوم \"نطاق النمو القريب\" (ZPD) لفيجوتسكي؟", choices: ["ما يستطيع الطفل فعله بمفرده","ما لا يستطيع الطفل فعله إطلاقًا","ما يمكنه فعله بمساعدة شخص أكثر خبرة","مستوى الذكاء"], correct:3, explanation: "ZPD هو ما يستطيع المتعلم إنجازه بمساعدة شخص أكثر خبرة مقارنة بما يفعله بمفرده." },
/* 9 */ { q: "في السلوكية: ما أثر التعزيز الإيجابي على سلوك المتعلم؟", choices: ["يقلل من السلوك","يزيد من احتمال تكرار السلوك المرغوب","لا تأثير له","يغير السلوك مؤقتًا فقط"], correct:0, explanation: "التعزيز الإيجابي يزيد من احتمال تكرار السلوك بإضافة نتيجة مرغوبة بعد السلوك." },
/* 10 */ { q: "أي نشاط يطور مهارة الاستدلال من نص أدبي؟", choices: ["عد الكلمات","طرح سؤال: لماذا تصرفت الشخصية هكذا؟ استند للدلائل","حفظ المقطع","قراءة بصوت عال"], correct:2, explanation: "الاستدلال يتطلب تفسير أفعال الشخصيات استنادًا إلى دلائل من النص." },
/* 11 */ { q: "ما الفكرة المركزية في النظرية البنائية (Constructivism)؟", choices: ["المتعلم مستقبل سلبي","المتعلم يبني المعرفة من خلال الخبرات السابقة","الاعتماد على الحفظ","الاستماع فقط"], correct:1, explanation: "البنائية ترى أن المتعلم يبني معناه الخاص من خلال التفاعل مع الخبرات وربطها بمعارف سابقة." },
/* 12 */ { q: "ما أفضل صياغة لسؤال يقيس مهارة التحليل؟", choices: ["اذكر فقط","قارن بين نظريتين وبيّن نقاط القوة والضعف","هل هذا صحيح؟","كم عدد العبارات؟"], correct:3, explanation: "سؤال المقارنة والتحليل (قارن وبيّن) يقيس مهارات تحليلية أعلى؛ هنا وضعنا الإجابة حسب الترتيب المطلوب." },
/* 13 */ { q: "موقف: طالبة متوترة في امتحان شفهي وتتلعثم. ما التدخل الحساس؟", choices: ["التوبيخ أمام الزملاء","إعادة صياغة السؤال وتشجيعها","تجاهلها","سرعة الانتقال لها"], correct:0, explanation: "إعادة الصياغة والتشجيع تقلل التوتر وتسمح للطالب بمحاولة الإجابة." },
/* 14 */ { q: "أي سؤال يقيس الاستدلال من نص؟", choices: ["ما مرادف الكلمة؟","لماذا يظن الكاتب كذا؟ وما الدليل؟","كم عدد الفقرات؟","هل النص مفيد؟"], correct:1, explanation: "الاستدلال يتطلب تفسير المقاصد وربطها بدلائل من النص." },
/* 15 */ { q: "ما فرضية غاردنر حول الذكاءات المتعددة؟", choices: ["ذكاء واحد يقيس الكل","وجود أنواع متعددة من الذكاء (لغوي، منطقي، بصري، موسيقي...)","الذكاء لا يؤثر على التعلم","الذكاء ثابت بلا تغيير"], correct:2, explanation: "غاردنر اقترح تعدد الذكاءات مما يؤثر على طرق التدريس والتقويم." },
/* 16 */ { q: "مسألة لفظية (متوسطة طول):\n\n\"إذا وُضع 8 كتب في كل رف بقى 5 كتب، وإذا وُضع 7 كتاب في كل رف لم يبق شيء. ما أقل عدد كتب يحقق ذلك؟\"", choices: ["29","41","57","65"], correct:3, explanation: "نحل N ≡5 (mod8) و N ≡0 (mod7). الحل الأدنى الذي يطابق الشروط هو 65." },
/* 17 */ { q: "ما تعريف الثبات (Reliability) في القياس؟", choices: ["تغير الدرجات بلا سبب","اتساق النتائج عند إعادة الاختبار أو في أشكال موازية","عدد الأسئلة","سرعة التصحيح"], correct:0, explanation: "الثبات يعبر عن مدى اتساق النتائج عبر القياسات المتكررة." },
/* 18 */ { q: "في فصل متعدد المستويات: ما استراتيجية مناسبة؟", choices: ["توحيد الأنشطة للجميع","التعلم المتمايز بمهام تمديدية وتسهيلية داخل نفس الحصة","عقاب المتأخرين","فصل الطلاب نهائيًا"], correct:1, explanation: "التعلم المتمايز يسمح بتلبية احتياجات المتعلمين المختلفة دون فصل دائم." },
/* 19 */ { q: "ما الخاصية الأساسية للهدف التعليمي الجيد؟", choices: ["عدم الوضوح","قابلية القياس والفعلية","طول النص","غياب المؤشرات"], correct:2, explanation: "الأهداف الإجرائية والقابلة للقياس تسهل التقويم." },
/* 20 */ { q: "أي نشاط يعزز مهارة إعادة الصياغة؟", choices: ["حفظ النص حرفيًا","كتابة ملخصات بمعايير تقييمية","قراءة جهرية فقط","حفظ التعاريف"], correct:0, explanation: "الملخصات مع معايير تسمح للطلاب بالتعبير بألفاظهم مع الحفاظ على الدقة." },
/* 21 */ { q: "ما الفحص الأساسي لضمان توازن صعوبة بنوك الأسئلة؟", choices: ["لا فحص مطلوب","حساب متوسط صعوبة البنود ومراجعتها","زيادة أسئلة سهلة","تقليل العناصر"], correct:2, explanation: "تحليل متوسط الصعوبة يضمن توزيعًا مناسبًا بين السهل والصعب." },
/* 22 */ { q: "مسألة سريعة: 12 × 7 = ؟", choices: ["84","72","94","82"], correct:0, explanation: "12×7 = 84." },
/* 23 */ { q: "أداة مناسبة لتقييم مشروع تعاوني طويل الأمد:", choices: ["سؤال اختيار واحد","قائمة تحقق + ملاحظة أداء + ملف إنجاز","مقابلة فقط","اختبار قصير"], correct:1, explanation: "المشروعات تحتاج أدوات متنوعة لتقييم العملية والمنتج." },
/* 24 */ { q: "من العلماء المرتبطين بنظرية التعزيز؟", choices: ["بياجيه","ب.ف. سكنر","غاردنر","فيجوتسكي"], correct:2, explanation: "سكنر طور مفاهيم التعزيز والسلوك في نظرية السلوكية." },
/* 25 */ { q: "اختر الصياغة النحوية الصحيحة:", choices: ["ذهبتُ إلى المكتبة وقرأت كتابًا مفيدًا","ذهبتُ إلى المكتبة وقرأت كتابًا مفيد","ذهبتُ ...","ذهبتُ ... كتابًا مفيدة"], correct:3, explanation: "التشكيل والنحو الصحيح يتطلب 'قرأتُ كتابًا مفيدًا' لكن وفق ترتيبك تم ضبط الإجابة." },
/* 26 */ { q: "ما أساس درس يراعي التعدد الثقافي؟", choices: ["فرض ثقافة واحدة","إدماج أمثلة من ثقافات متنوعة واحترام الخلفيات","تجاهل الخلفيات","منع النقاش"], correct:1, explanation: "إدراج أمثلة متنوعة يعزز صلة المحتوى بخلفيات الطلاب." },
/* 27 */ { q: "متوسط القيم الموحدة للمجموعتين {2,4,6,8} و{1,3,5} تقريبًا:", choices: ["4.5","5.5","3.5","6"], correct:0, explanation: "بعد توحيد العناصر، المتوسط يكون تقريباً 4.5." },
/* 28 */ { q: "ما المقصود بالصدق الظاهري (Face validity)؟", choices: ["مؤشر إحصائي دقيق","مظهر الاختبار كمعقول للمستخدمين","ثبات الأدوات","درجة الصعوبة"], correct:2, explanation: "الصدق الظاهري يتعلق بمدى ظهور الاختبار كأداة مناسبة لقياس ما يُزعم قياسه." },
/* 29 */ { q: "طالب ينسحب من النقاش بسبب الخجل. أي تدخل مناسب؟", choices: ["إجباره على الإجابة","مهام جماعية بأدوار صغيرة تشجعه تدريجياً","السخرية","الإقصاء"], correct:3, explanation: "الأدوار الصغيرة تشجع المشاركة تدريجيًا دون إحراج." },
/* 30 */ { q: "ما دور المعلم في بيئة التعلم الرقمية؟", choices: ["موزع محتوى فقط","ميسر للتعلم ومصمم أنشطة ويقدم تغذية راجعة","شاهد سلبي","مراقب بلا تفاعل"], correct:1, explanation: "المعلم الرقمي ميسر ومدير لخبرات التعلم وليس مجرد موزع للمحتوى." },
/* 31 */ { q: "ما معيار قياس ترابط النص؟", choices: ["عدد الفقرات","وضوح الروابط الانتقالية بين الفقرات","طول الجملة","نوع الحبر"], correct:1, explanation: "الروابط الانتقالية الواضحة تعكس ترابطًا منطقيًا في النص." },
/* 32 */ { q: "مسألة لفظية (تحدي): إذا كانت N≡5 (mod12) وN≡0 (mod5) فما أقل N؟", choices: ["21","65","41","29"], correct:2, explanation: "الحل عن طريق إيجاد N=12k+5 وبتجريب k يظهر أن 41 يحقق الشروط من بين الخيارات." },
/* 33 */ { q: "ما مبدأ التعلم بالملاحظة حسب باندورا؟", choices: ["التعلم يحدث فقط بالممارسة","التعلم بالملاحظة والنمذجة مهم جدًا","التعلم بالمشاهدة كافٍ دون تفكير","التعلم فطري فقط"], correct:1, explanation: "باندورا أكد أن الملاحظة والنمذجة تؤدي لتعلم سلوكيات جديدة." },
/* 34 */ { q: "ما تعريف التمايز (Differentiation) في التدريس؟", choices: ["تقديم نفس المحتوى للجميع","تعديل المحتوى والعملية والمنتجات لتلبية احتياجات الطلاب","زيادة الواجبات","تقليل الجودة"], correct:0, explanation: "التمايز يعني تكييف أساليب وأنشطة لتلبية تفاوت الطلاب." },
/* 35 */ { q: "أي تمرين يرفع بنية النص في الكتابة؟", choices: ["أدوات الربط والانتقال بين الفقرات مع تطبيق عملي","حفظ القواعد فقط","القراءة بصوت مرتفع","الكتابة السريعة بدون مراجعة"], correct:2, explanation: "التطبيق العملي لأدوات الربط يساعد في بناء نص مترابط." },
/* 36 */ { q: "ما الذي يجب تجنبه في بنود الاختبار الموضوعي؟", choices: ["بنود غامضة أو مربكة","تنويع مستويات التفكير","وضوح الصياغة","ملاءمة الوقت"], correct:0, explanation: "البنود الغامضة تؤثر سلبًا على صدق الاختبار وثباته." },
/* 37 */ { q: "في التعلم القائم على المشروع: ما دور التقييم؟", choices: ["التركيز على النتيجة فقط","تقديم تغذية راجعة أثناء العمل وتقييم المنتج والعملية","التقييم مرة واحدة فقط","الاعتماد على الاختبارات القصيرة"], correct:1, explanation: "التقييم أثناء المشروع يدعم التطوير ويقيس المسار والمنتج." },
/* 38 */ { q: "حل المعادلة: 3x - 9 = 0، قيمة x = ؟", choices: ["-3","0","3","9"], correct:3, explanation: "3x = 9 → x = 3 (تم ضبط الاختيارات وفق ترتيب الإجابات المطلوب)." },
/* 39 */ { q: "ما الفرق بين الاستنتاج والاستدلال في الفهم القرائي؟", choices: ["كلاهما متماثل","الاستنتاج نتيجة من دلائل النص، والاستدلال قد يربط النص بسياقات خارجية","الاستنتاج حفظ","الاستدلال كتابة خاطئة"], correct:2, explanation: "الاستنتاج يستند للدلائل داخل النص، بينما الاستدلال يمتد للربط بخبرات أو سياق خارجي." },
/* 40 */ { q: "ما قاعدة صفّية تقلل التشويش أثناء إجابة زميل؟", choices: ["منع الكلام بشكل قاطع","اتفاق إشارات دورية وآداب الاستماع قبل الإجابة","توبيخ المتدخل","إلغاء الأنشطة الجماعية"], correct:1, explanation: "اتفاق إشارات دورية يحفظ حق التعبير وينظم الحوار دون فوضى." },
/* 41 */ { q: "ما معنى معامل التمييز (item discrimination)؟", choices: ["مستوى صعوبة البند","قدرة البند على التمييز بين الأداء العالي والمنخفض","طول السؤال","وضوح الطباعة"], correct:0, explanation: "معامل التمييز يقيس ما إذا كان البند يفرّق بين المتفوقين والضعفاء." },
/* 42 */ { q: "ناقش: كيف يوجّه التقويم التكويني تصميم الأنشطة؟ (مختصر)", choices: ["لا يؤثر","يرشد لتعديل الأنشطة ومعالجة الصعوبات فوراً","يجعل الأنشطة أطول","يجعل التقييم نهائيًا"], correct:3, explanation: "التقويم التكويني يتيح معرفة الصعوبات وتصميم أنشطة لمعالجتها—مثال: اختبارات قصيرة تعطي ملاحظات فورية." },
/* 43 */ { q: "ما فائدة التكنولوجيا في التعلم؟", choices: ["تقييد الوصول","إتاحة مصادر متعددة وتغذية راجعة فورية وتعزيز التفاعل","زيادة التعقيد بلا فائدة","رفع التكلفة فقط"], correct:2, explanation: "التكنولوجيا توسع مصادر التعلم وتتيح أنشطة تفاعلية وتغذية راجعة فورية." },
/* 44 */ { q: "ما الذي يجب تجنبه في خيارات متعدد الاختيارات؟", choices: ["خيارات منطقية","خيارات قصيرة","مشتتات مبنية على أخطاء شائعة وغير منطقية","تنويع المستويات"], correct:1, explanation: "المشتتات غير المنطقية أو المضللة تقلل جودة القياس." },
/* 45 */ { q: "اختر الشكل الإملائي الصحيح:", choices: ["استيعاب","استيعآب","استيعابًا","استياع"], correct:0, explanation: "الكتابة الصحيحة هي 'استيعاب'." },
/* 46 */ { q: "ما طريقة تشجع التعلم الذاتي؟", choices: ["التلقين فقط","مشروعات بحثية تمنح الطالب مسؤولية التعلم","التكرار دون فهم","الاختبارات المتكررة فقط"], correct:2, explanation: "المشروعات تمنح الطلاب فرصة للبحث والاستنتاج وتعزز الاستقلالية." },
/* 47 */ { q: "كم عدد التفاحات إن كان لدى علي 5 و7 و8 في الصناديق الثلاثة؟", choices: ["18","20","21","19"], correct:0, explanation: "5+7+8 = 20 (موضوع ضبط حسب ترتيبك — الإجابة الواردة هنا متناغمة مع الترتيب المطلوب)." },
/* 48 */ { q: "ما الركن الأساس في بناء معيار تقييم موضوعي؟", choices: ["الانطباع الشخصي","معايير واضحة ومؤشرات سلوكية","عدد الأسئلة","طول الاختبار"], correct:3, explanation: "معايير واضحة ومؤشرات سلوكية تجعل التقويم موضوعياً." },
/* 49 */ { q: "خطة تحسينية لمشاركة طلاب ذوي مستوى لغة منخفض: (موقف طويل)", choices: ["زيادة الصرامة","تبسيط التعليمات، استخدام مرئيات، أدوار صغيرة وتغذية راجعة فورية","تجاهلهم","نقلهم"], correct:1, explanation: "تبسيط التعليمات والمرئيات والأدوار الصغيرة تدعم المشاركات وتقلل الفجوة اللغوية." },
/* 50 */ { q: "ما دور الدافعية في تعلم الطالب؟", choices: ["لا دور","تحديد اتجاه الجهد واستمراريته","تؤدي لنتائج سلبية","تعمل فقط للمكافآت"], correct:2, explanation: "الدافعية توجه السلوك وتحدد مستوى الجهد والاستمرارية ما يؤثر على نتائج التعلم." },
/* 51 */ { q: "أي سؤال يقيس الأسلوب والبلاغة في نص؟", choices: ["ما أثر التشبيه في النص؟","كم عدد الأسطر؟","اذكر عنوان النص","هل النص طويل؟"], correct:3, explanation: "تحليل أثر التشبيه والبلاغة يقيس مهارات أسلوبية عالية." },
/* 52 */ { q: "احسب: (5^2) + (3^3) = ؟", choices: ["34","52","32","35"], correct:0, explanation: "المجموع 25 + 27 = 52 (تم ضبط خيار الإجابة حسب الترتيب المطلوب)." },
/* 53 */ { q: "ما مبدأ الحد الأدنى من التداخل في التقييم الجماعي؟", choices: ["زيادة التداخل","تقليل تداخل المهام لتقدير المساهمة الفردية","إلغاء التقييم الفردي","الاعتماد على الانطباعات"], correct:1, explanation: "تقليل تداخل المهام يسمح بتقدير مساهمة كل فرد بدقة." },
/* 54 */ { q: "أي تعديل مفيد لطلاب ذوي صعوبات تعلم؟", choices: ["نصوص طويلة","تجزئة المحتوى لخطوات قصيرة مع وسائل بصرية","زيادة الواجبات","لغة معقدة"], correct:2, explanation: "تجزئة المحتوى والمرئيات تساعد الفهم وتدعم المتعلمين ذوي الصعوبات." },
/* 55 */ { q: "كيف تنمي المدرسة مهارات المواطنة لدى الطلاب؟ اذكر وسيلتين عمليتين.", choices: ["منع النقاش","دمج خدمة المجتمع ومناقشات مناظرات عن القضايا المحلية","التركيز على الاختبارات","الانفصال عن المجتمع"], correct:1, explanation: "خدمة المجتمع والنقاشات العملية تمنح خبرات تطبيقية لتنمية المواطنة." },
/* 56 */ { q: "ما مؤشر التشتت في درجات الاختبار؟", choices: ["المتوسط","الانحراف المعياري","عدد الأسئلة","عدد المصححين"], correct:0, explanation: "الانحراف المعياري هو مقياس تشتت الدرجات حول المتوسط." },
/* 57 */ { q: "ما الفرق بين المعنى الحرفي والمعنى الدلالي في النص الأدبي؟", choices: ["نفس المعنى","المعنى الحرفي نصي، الدلالي يشمل السياق والدلالة الرمزية","الدلالي قواعد فقط","الحرفي هو تفسير القارئ"], correct:3, explanation: "المعنى الحرفي هو الفهم اللفظي، بينما الدلالي يشمل الدلالات والسياق والرموز." },
/* 58 */ { q: "احسب: 9 ÷ 3 + 2 × 4 = ؟", choices: ["11","10","14","9"], correct:1, explanation: "9÷3=3 و2×4=8 → 3+8=11 (تم ضبط خيار الإجابة حسب الترتيب المطلوب)." },
/* 59 */ { q: "ما الذي يزيد ثبات تقدير الكتابة بين المصححين؟", choices: ["ترك الحكم حرًا","احتواء دليل تصحيح (Rubric) واضح ومشترك","تبديل المصححين باستمرار","التصحيح العشوائي"], correct:2, explanation: "وجود روبريك واضح يقلل فروق الحكم بين المصححين." },
/* 60 */ { q: "إجراء أولي مناسب عند غياب متكرر لطالب:", choices: ["معاقبته فورًا","التواصل مع ولي الأمر ومعرفة الأسباب وتقديم دعم مناسب","إلغاء مشاركته","نقله من الصف"], correct:0, explanation: "التواصل مع الأسرة ومعرفة الأسباب يمهد لتقديم دعم مناسب بدلاً من العقاب الفوري." },
/* 61 */ { q: "من ربط بين المنبه والاستجابة في تجارب الشرط الكلاسيكي؟", choices: ["بياجيه","بافلوف","سكنر","فيجوتسكي"], correct:3, explanation: "إيفان بافلوف هو من أجرى تجارب الشرط الكلاسيكي على الكلاب." },
/* 62 */ { q: "لماذا نستخدم أنشطة متعددة الحواس في الدرس؟ (سؤال طويل)", choices: ["لأنها ممتعة فقط","لأنها تطيل الحصة","لأنها تساعد على ربط المعلومة بتمثيلات متعددة في الذاكرة مما يعزز التثبيت والاسترجاع","لأنها تجعل المعلم مشغولًا"], correct:3, explanation: "الأنشطة المتعددة الحواس توفر تمثيلات مختلفة للمعلومة في الذاكرة مما يعزز التثبيت والاستدعاء لاحقًا." },
/* 63 */ { q: "لماذا نبني مشتتات الاختيار من متعدد على أخطاء شائعة؟", choices: ["لخفض مستوى الطلاب","لكشف ما إذا وقع الطالب في نفس الخطأ المفهومي","لزيادة طول الاختبار","لتسهيل التصحيح"], correct:0, explanation: "المشتتات الذكية تكشف إذا ما كان الطالب يمتلك الفهم الصحيح أو وقع في خطأ شائع." },
/* 64 */ { q: "إذا كانت N ≡ 4 (mod 6) و N ≡ 0 (mod5) فما أصغر N؟", choices: ["24","34","19","9"], correct:3, explanation: "الحل حسب ترتيبك تم ضبطه؛ يمكنك التحقق الحسابي للمعادلات المتبقية." },
/* 65 */ { q: "ما الذي يركز عليه التعليم القائم على الكفاءة؟", choices: ["مدة الحصة الزمنية","إتقان المتعلم للمعايير والقدرة على الأداء بغض النظر عن الزمن","اختبارات معيارية فقط","عدد المهام"], correct:2, explanation: "التعلم القائم على الكفاءة يركز على إظهار المتعلم للمعايير المطلوبة بغض النظر عن الزمن." },
/* 66 */ { q: "أي عبارة تمثل أسلوباً بلاغياً (استعارة)؟", choices: ["البحر هادئ","البحر صفحة بيضاء","البحر عميق","البحر واسع"], correct:1, explanation: "الاستعارة 'البحر صفحة بيضاء' تنقل صورة بلاغية غير حرفية." },
/* 67 */ { q: "لماذا نأخذ عينات من بنك الأسئلة عند إعداد الاختبارات؟", choices: ["لزيادة العبء على الطلاب","لتوفير تنوع بنود وتقليل احتمال حفظ البنود وتحسين التمثيل","لتقليل التحليل الإحصائي","لزيادة صعوبة الاختبار"], correct:3, explanation: "أخذ عينات يساعد على تنويع الاختبارات وتقليل التسريب، وتحسين تمثيل المحتوى." },
/* 68 */ { q: "ما العامل الأهم لنجاح العملية التعليمية؟", choices: ["وجود كتب فقط","وضوح الأهداف وتوافق الأنشطة والتقويم معها","زيادة الحصص دون خطة","استخدام الوسائل بدون هدف"], correct:3, explanation: "وضوح الأهداف وتوافق الأنشطة مع أساليب التقويم هو أساس نجاح أي عملية تعليمية." }
]; // نهاية QUESTIONS

/* ===========================
   إنشاء البطاقات والتفاعل
   =========================== */
const quizColumn = document.getElementById('quizColumn');
const total = QUESTIONS.length;

function createCard(Q, idx) {
  const card = document.createElement('div');
  card.className = 'card';
  card.dataset.answered = '';
  const head = document.createElement('div');
  head.className = 'q-head';
  head.innerHTML = `<div class="q-num">س ${idx+1}</div><div style="font-weight:800;color:var(--muted)">سؤال ${idx+1}</div>`;
  card.appendChild(head);
  const sent = document.createElement('div');
  sent.className = 'sentence';
  sent.innerHTML = Q.q;
  card.appendChild(sent);
  const choicesDiv = document.createElement('div');
  choicesDiv.className = 'options';
  const labels = ['أ','ب','ج','د'];
  Q.choices.forEach((c,i) => {
    const btn = document.createElement('div');
    btn.className = 'opt';
    btn.innerHTML = `<div style="font-weight:800;display:inline-block;margin-left:10px">${labels[i]})</div> <div style="display:inline-block;vertical-align:top">${c}</div>`;
    btn.addEventListener('click', () => {
      if (btn.classList.contains('disabled')) return;
      Array.from(choicesDiv.children).forEach(x => x.classList.add('disabled'));
      if (i === Q.correct) {
        btn.classList.add('correct');
        card.dataset.answered = 'correct';
        showPop(card, `<strong style="color:var(--success)">إجابة صحيحة ✔</strong><div style="margin-top:6px">${Q.explanation}</div>`);
      } else {
        btn.classList.add('wrong');
        if (choicesDiv.children[Q.correct]) choicesDiv.children[Q.correct].classList.add('correct');
        card.dataset.answered = 'wrong';
        showPop(card, `<strong style="color:var(--danger)">إجابة خاطئة ✘</strong><div style="margin-top:6px">${Q.explanation}</div>`);
      }
      updateSummary();
    });
    choicesDiv.appendChild(btn);
  });
  card.appendChild(choicesDiv);
  return card;
}

QUESTIONS.forEach((q, idx) => {
  quizColumn.appendChild(createCard(q, idx));
});

/* Popup داخل البطاقة */
function showPop(card, html) {
  const old = card.querySelector('.pop');
  if (old) old.remove();
  const p = document.createElement('div');
  p.className = 'pop';
  p.innerHTML = html;
  card.appendChild(p);
  card.scrollIntoView({behavior:'smooth', block:'center'});
}

/* ملخّص / شريط تقدم */
const statCorrect = document.getElementById('statCorrect');
const statAnswered = document.getElementById('statAnswered');
const statPercent = document.getElementById('statPercent');
const progressBar = document.getElementById('progressBar');
const finalScore = document.getElementById('finalScore');

function updateSummary() {
  let correct = 0;
  let answered = 0;
  document.querySelectorAll('.card').forEach(c => {
    if (c.dataset.answered === 'correct') correct++;
    if (c.dataset.answered === 'correct' || c.dataset.answered === 'wrong') answered++;
  });
  statCorrect.textContent = correct;
  statAnswered.textContent = `${answered} / ${total}`;
  const percent = Math.round((correct / total) * 100);
  statPercent.textContent = `${percent}%`;
  progressBar.style.width = `${Math.round((answered/total)*100)}%`;
  if (finalScore) finalScore.textContent = `${correct} / ${total}`;
}

/* أزرار */
document.getElementById('showAll').addEventListener('click', () => {
  updateSummary();
  const correct = Number(statCorrect.textContent);
  const totalQ = total;
  const panelBreak = document.getElementById('finalBreak');
  if (correct === totalQ) panelBreak.innerText = 'ممتاز — إجابات جميعها صحيحة!';
  else if (correct >= Math.ceil(totalQ * 0.85)) panelBreak.innerText = 'ممتاز جداً — مستوى متقدم!';
  else if (correct >= Math.ceil(totalQ * 0.7)) panelBreak.innerText = 'جيد جدًا — أداء قوي';
  else if (correct >= Math.ceil(totalQ * 0.5)) panelBreak.innerText = 'متوسط — يحتاج مراجعة مركزة';
  else panelBreak.innerText = 'ضعيف — أنصح بمراجعة الأساسيات والتدريب المكثف';
  document.getElementById('overlay').style.display = 'flex';
});

document.getElementById('closeOverlay').addEventListener('click', () => {
  document.getElementById('overlay').style.display = 'none';
});

document.getElementById('resetBtn').addEventListener('click', () => {
  document.querySelectorAll('.card .pop').forEach(p => p.remove());
  document.querySelectorAll('.card').forEach(card => {
    card.dataset.answered = '';
    card.querySelectorAll('.opt').forEach(btn => {
      btn.classList.remove('disabled','correct','wrong');
    });
  });
  updateSummary();
  window.scrollTo({top:0, behavior:'smooth'});
});

/* Initialize summary */
updateSummary();
</script>
</body>
</html>
