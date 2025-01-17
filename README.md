# מסמך סיכום
## ניתוח הפרוטוקולים

בהמשך יוצג:
- ניתוח הפרוטוקולים
- ניתוח חולשות בפרוטוקולים והצעות לתיקונים
- החולשות מתוך ספר הלימוד

### ניתוח הפרוטוקולים

בפרוטוקול, משתמשים בהצפנה סימטרית באמצעות CBC-AES להצפנת המסרים ובהצפנה אסימטרית באמצעות RSA להצפנת המפתח.

#### הצפנה סימטרית (CBC-AES)
AES (Advanced Encryption Standard):<br>
AES הוא אלגוריתם הצפנה מתקדם ונפוץ המשמש להצפנת מסרים בצורה סימטרית.<br>
<br>גודל המפתח: אורך המפתח הוא 128 ביט, מה שהוא רגיל לגרסאות AES-128.
<br>
הצפנה: CBC (Cipher Block Chaining): זהו מצב צפנה סימטרי המשמש להבטחת המסרים מתוך הקריפטוגרפיה בבלוקים. <br>כל בלוק  מיישם הצפנה עם ביטי המסר של הבלוק הקודם.<br>

IV (Initialization Vector): ערך אקראי ראשוני המתווסף לבלוק הראשוני ובכך בכל תהליך הצפנה, נעשה שימוש ב-IV כדי להבטיח שלא יתקבל אותו תוצאה עבור כל בלוק מסר זהה. בהקצאה קבועה כמו במקרה שהוזכר, ה-IV מאופס תמיד.<br>


#### הצפנה אסימטרית (RSA)
RSA (Rivest–Shamir–Adleman):<br>
אלגוריתם אסימטרי: RSA הוא אלגוריתם אסימטרי המשתמש בזוג מפתחות, מפתח ציבורי ומפתח פרטי.<br>
גודל המפתח: בפרוטוקול הנוכחי נעשה שימוש במפתחות עם אורך של 1024 ביט.<br>
<br>
הצפנת מפתח: בפרוטוקול זה, יתקבל מפתח AES בצורה סימטרית ויצפן באמצעות RSA כלומר המפתח יוצפן באמצעות RSA.<br>


### ניתוח חולשות בפרוטוקולים והצעות לתיקונים:

- **אורך המפתח ב-RSA:**
<br>חולשה: אורך המפתח של 1024 ביט יכול להיות פחות מאובטח בימינו.<br>
תיקון: ממליץ לשקול הגברת אורך המפתח, לפחות ל-2048 ביט או אף יותר.<br>


- **שימוש ב-IV מאופס:**
<br>חולשה: שימוש ב-IV מאופס באופן קבוע עשוי להיחשב כחולשה במקרים בהם משתמשים באותו מפתח AES חוזר וחוזר.<br>
תיקון: שימוש ב-IV ייצמד למפתח AES ויהיה ייחודי לכל תהליך הצפנה, מה שימנע תופעת CBC replay ויבטיח עמידות נוספת.<br>


- **קביעות בדיקה ועדכון באופן קבוע :**
<br>אלגוריתם RSA משתמש בזוג מפתחות. חשוב לבדוק בקביעות את עמידותם ולעדכן אורך המפתח ואת האלגוריתם עצמם כאשר נדרש.<br>
תיקון: יש לעדכן ולשדרג באופן קביעות את אורך המפתח ולכלול תקציב אבטחה שמתעדכן עם הזמן.<br>

- **קשר בין המפתחות:**
  <br>שימוש ב-RSA להצפנת מפתח ה-AES יוצר קשר בין המפתחות. אם מפתח ה-RSA נחשף, ייתכן שהמפתח ה-AES יהיה חשוף גם.<br>
תיקון: ניתן לשקול יצירת מנגנון נוסף שיאבטח את מפתח ה-AES בדרך אחרת ממפתח ה-RSA. זה יכול להכליל שיטה של סיסמאות, אימות דו-שלבי, או שיטה אחרת שתפחית התקפות פוטנציאליות.<br>

- **במפתחות ואימות:**
  <br>חשוב לוודא שמפתחות ה-RSA וה-AES מנוהלים בצורה בטוחה, עם ניהול נכון ובזמן אמת.<br>
תיקון: מניהול מפתחות יציב ובטוח כדי למנוע חשיפת מפתחות בזמן רציני. השגחה על פעילות מפתחות, תקצוב במדיניות אבטחה, ואמינות בין המשתמשים.<br>

- **זמן וביצועים:**
  <br>שיטה זו יכולה לכלול זמני קריפטוגרפיה ארוכים ולהשפיע על ביצועי המערכת, במיוחד כאשר מטרת השימוש היא ביצוע מהיר.<br>
תיקון: שיפור ביצועים יכול לכלול שימוש במפתחות קצרים יותר לפעמים בקבוצה מסוימת, או אופטימיזציות באלגוריתמים. אך כדאי להשקיע בשלב תכנונים נוספים לקבלת ביצועים מתקדמים.<br>


- **העברת המפתח האסימטרי:**
  <br>הצורך להעביר את המפתח האסימטרי בין הלקוח והשרת מצמיד את המערכת לאפשרות של חשיפת המפתח במהל  ך ההעברה.<br>
התקפה פוטנציאלית: התקפות מתוך חשש גישה למפתח האסימטרי, שבתורו יכול להוביל לחשיפת מפתח ההצפנה הסימטרית.<br>
תיקון: שימוש בפרוטוקולים מתקדמים יותר להעברת מפתחות יכול לספק שכבת אבטחה נוספת, לדוג', בשימוש ב Diffie-Hellman.<br>

- **ניהול מפתחות:**
  <br>אם יש בעיות בניהול המפתחות הסימטריים והאסימטריים, זה יכול לביא לחשיפת המפתח ולפרצה במערכת.<br>
התקפה פוטנציאלית: ניהול רע של מפתחות יכול לספק לגורמים מתוך חשיפה גישה למפתחות באופן לא רשותי.<br>
תיקון: ניהול אבטחתי של המפתחות, שכולל זיהוי ואימות של המשתמשים וניהול מפתחות באמצעות טכנולוגיות בטוחות יותר ומתן המפתחות לגורמים מורשים בלבד.<br>

- **אמינות המפתח:**
<br>אם מפתח ההצפנה הסימטרי נחשף או אם יש פגיעה באמינותו, זה יכול לפגוע באבטחת המערכת.<br>
התקפה פוטנציאלית: התקפות כדי לשבור את האמינות של מפתח ההצפנה הסימטרי עשויות להתרחש.<br>
תיקון: שימוש במפתחות אמינים, שמאובטחים באופן יציב ונבדקים באופן תדיר, ואם יש חשש לאמינותם - החלפתם במפתחות חדשים.<br>


- **באובדן מפתח פרטי:**
  <br>אם משתמש מאבד את המפתח הפרטי שמשמש להצפנת המפתח הסימטרי, ייתכן שלא יהיה ניתן לגשת למידע המוצפן.<br>
התקפה פוטנציאלית: אם מישהו מצליח לראות את המפתח הפרטי או לגשת אליו, ייתכן שיהיה ניתן לפענח קבצים כדי לגשת למידע המוצפן.<br>
תיקון: חשוב למצוא דרכים לאבטחה ואימות רכיבים כמו מפתחות פרטיים כך שהם לא יאבדו, כמו גם למצוא דרכים להתמודד עם אובדן מפתחות בצורה בטוחה.<br>

- **שקיפות האלגוריתם :**
  <br>אם האלגוריתם אינו שקוף ואין אפשרות לבדיקה חיצונית של הצפנה/פענוח, יתכן שיהיה קשה לזהות ולתקן חולשות.<br>
הצעה לתיקון: שימוש באלגוריתם צפנה כללי ומוכר, עם הסברים שקופים על תהליכי הצפנה ופענוח.<br>


- **בעיות של חולשה בהצפנה:**
<br>הקוד יכול לחשוף חולשות פוטנציאליות בכללי היצירה, הטיפול במפתחות, ואבטחת ההצפנה.<br>
תיקון: Static Analysis ניתוח סטטי של הקוד הוא חשוב ויכול לשרת ככלי נוסף לאיתור ותיקון של חולשות בפרוטוקול.<br>




### החולשות מתוך ספר הלימוד
| סוג החולשה או התקפה           | תיאור                                                                                                    |
|-----------------------------|---------------------------------------------------------------------------------------------------------|
| Heap Spraying               | התקפה שבה יתקף מופע של ++C זר מכתובת זיכרון מוקצה לערמה, ויכתוב קוד בחלק מהזיכרון הזה.                 |
| Return Oriented Programming | התקפה שבה התוקף משתמש ברצפים של קוד קיימים במערכת כדי להריץ קוד זר, ומסתמך על פונקציות קיימות במערכת.  |
| Buffer Overflow             |  חולשה באבטחה שבה התוקף יכול להכניס יותר מדי נתונים למערכת, על ידי כתיבה יתרה לתאי הזיכרון מחוץ לגבולות המוגדרים עבור משתנה מסויים. |

