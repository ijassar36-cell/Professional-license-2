<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />

  <title>محاكي الرخصة المهنية — إعداد المعلم: فهد الخالدي</title>

  <link rel="stylesheet" href="style.css" />
</head>

<body>

  <div class="container">

    <h1 class="title">محاكي الرخصة المهنية</h1>
    <p class="credit">إعداد المعلم: فهد الخالدي</p>

    <div id="quiz-box">
      <div id="question-text"></div>

      <div id="options"></div>

      <button id="nextBtn" disabled>السؤال التالي</button>
    </div>

  </div>

  <!-- نافذة الشرح -->
  <div id="explain-popup">
    <div class="popup-content">
      <p id="explain-text"></p>
      <button id="close-popup">إغلاق</button>
    </div>
  </div>

  <script src="questions.js"></script>
  <script src="script.js"></script>

</body>
</html>
