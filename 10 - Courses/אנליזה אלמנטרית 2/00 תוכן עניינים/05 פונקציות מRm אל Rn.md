## פונקציות מR^m אל R^n מבוא מפרק 04

שני דרכים אפשריות לבטא 
$$
\underline{f}(u_1, \dots, u_m) = 
\begin{pmatrix}
f_1(u_1, \dots, u_m) \\
f_2(u_1, \dots, u_m) \\
\vdots \\
f_n(u_1, \dots, u_m)
\end{pmatrix}
= 
\begin{pmatrix}
x_1(u_1, \dots, u_m) \\
x_2(u_1, \dots, u_m) \\
\vdots \\
x_n(u_1, \dots, u_m)
\end{pmatrix}
$$
הצגנו כמה מקרים פרטיים של פונקציות מהצורה

- למדנו בעבר על [[פרמטריזציה]] של עקומות ב$\mathbb{R}^n$
[[מסילות ממועד א' תשפה]] 
בעצם מה שעשינו הוא פונקציה מהצורה 
$$
	\underline{g}: \mathbb{R} \to \mathbb{R}^n
$$כלומר $m=1$  


- בתחילת הפרק (פרק [[03 פונקציות מRm לR]]) עסקנו בפונקציות כאשר $n=1$

נציג כמה מקרים של פונקציות מהצורה
$$
	\underline{g}: \mathbb{R}^n \to \mathbb{R}^m
$$
-  כאשר 
   $$
   	\underline{f}: \mathbb{R}^2 \to \mathbb{R}^2
   $$
  $$
  	n = m =2
  $$
 מעבר אל [[קוארדינטות פולאריות]] הוא בעצם פונקציה מהצורה הזו 
$$
	\underline{g}(u,v) = \begin{pmatrix}
	x \\
	y
	\end{pmatrix} = \begin{pmatrix}
	u\cos v \\
	u\sin v
	\end{pmatrix}
$$

-  כאשר 
   $$
   	\underline{f}: \mathbb{R}^3 \to \mathbb{R}^3
   $$
  $$
  	n = m = 3
  $$
ראינו כי מעבר ל[[קוארדינטות כדוריות]] הוא בעצם פונקציה מהצורה הזו
$$
	\underline{f} = \begin{pmatrix}
	x \\
	y \\
	z \\
	
	\end{pmatrix} = \begin{pmatrix}
	r\sin \phi \cos \theta \\
	r\sin \phi \sin \theta \\
	r\cos \phi
	\end{pmatrix}
$$

-    כאשר 
     $$
     	\underline{f}: \mathbb{R}^3 \to \mathbb{R}^2
     $$$$
   	n = 3, m=2
   $$
פרמטריזציה של החלק העליון של ספירת היחידה היא פונקציה מהצורה הזו 
- $$
  	\underline{f}=  \begin{pmatrix}
	x \\
	y \\
	z \\
	
	\end{pmatrix}  
	= \begin{pmatrix}
	\sin \phi \cos \theta \\
	\sin \phi \sin \theta \\
	\cos \phi
	\end{pmatrix} \; ; \quad \left\{  (\phi,\theta)| 0  \leq \phi  \leq \frac{\pi}{2},0 \leq \theta \leq 2\pi  \right\}
  $$
  ![[Pasted image 20260706173413.png]]
  לאחר מכן חזרנו והוספנו 
  [[רציפות של פונקציה רבת משתנים]]
וחיזקנו גם 
[[5.1.2 דיפרנציאביליות]] בהגדרה כללית יותר
שאלנו מה ניתן לעשות עם דיפרנציאביליות של פונקציה מ 
$$
	\underline{f}: \mathbb{R}^m \to \mathbb{R}^n
$$
ניתן
למצוא [[קירוב לינארי#פונקציה רבת משתנים| קירוב לינארי של פונקציה מרובת משתנים]]
בנוסף נוכל גם למצוא שטח
נניח 
$$
	m=n =2
$$
נסמן 
$$
	\underline{f}= \begin{pmatrix}
	f_{1}(u,v) \\
	f_{2}(u,v)
	\end{pmatrix}=\begin{pmatrix}
	x(u,v) \\
	y(u,v)
	\end{pmatrix}
$$
![[Pasted image 20260706182405.png]]
נרצה למצוא את השטח בקירוב לינארי כך שכשנשאיף את $dudv$ כלומר את השגיאה בקירוב לאפס נקבל את השטח בדיוק.
השטח $x,y$ בקירוב לינארי יהיה שטח המקבילית המוגדרת 
$$
= \frac{\partial \underline{f}}{\partial u} du, \frac{\partial \underline{f}}{\partial v} dv
$$
כלומר 
$$
\left| \det \left( \frac{\partial \underline{f}}{\partial u} du \ \vdots \ \frac{\partial \underline{f}}{\partial v} dv \right) \right| = \left| \det \left( \frac{\partial \underline{f}}{\partial u} \ \vdots \ \frac{\partial \underline{f}}{\partial v} \right) \right| du \, dv
$$
שהרי ה[[דטרמיננטה]] היא שטח המקבילית של ההעתקה, 
(זה קצת מוזר כי לכאורה ההעתקה לא לינארית אלא שמסתבר שבסביבה מספיק קטנה ההעתקה מתנהגת כמו פונקציה לינארית וזה נובע מהיותה דיפרנציאבילית!)
המטריצות הללו של הפונקציה נקראות מטריצת הנגזרת והדטרמיננטה שלהן במידה והן דיפרנציאביליות היא בעלת משמעות גיאומטרית בנוגע ליחס שינוי השטח או הנפח.
נסמן את מטריצה הנגזרת של פונקציה להיות
$$
	D\underline{f}
$$
ונגדיר את הדטרמיננטה שלה להיות 
ה[[יעקוביאן]]
באמצעותו ניתן גם לבטא את השטח כאמור 
$$
	dA = \left| \frac{\partial(x,y)}{\partial(u,v)} \right| du \, dv
$$
ניתן להרחיב גם ממד למעלה ולעלות לנפח ב$x,y,z$ כאשר 
$$
	m=n=3
$$
![[Pasted image 20260706185045.png]]
נפח המקבילות הנפרש על ידי הווקטורים 
$$
\frac{\partial \underline{f}}{\partial u} du, \frac{\partial \underline{f}}{\partial v} dv, \frac{\partial \underline{f}}{\partial w} dw
$$
כלומר
$$
= \left| \det \left( \frac{\partial \underline{f}}{\partial u} du \ \vdots \ \frac{\partial \underline{f}}{\partial v} dv \ \vdots \ \frac{\partial \underline{f}}{\partial w} dw \right) \right| = \left| \det \left( \frac{\partial \underline{f}}{\partial u} \ \vdots \ \frac{\partial \underline{f}}{\partial v} \ \vdots \ \frac{\partial \underline{f}}{\partial w} \right) \right| du \, dv \, dw
$$
$$
= |\det D\underline{f}| du \, dv \, dw
$$
כלומר ה[[יעקוביאן]] ב$\mathbb{R}^3$ 

---

ניזכר כי הקוארדינטות הפולאריות הקוטביות והגליליות הן בעצם פונקציות נבטא את השטח והנפח של כל אחת מהן.
תרגיל טוב הוא לעשות את המעברים הללו בעצמך

**א) קוטביות**
$$
\begin{pmatrix} x \\ y \end{pmatrix} = \begin{pmatrix} r \cos \theta \\ r \sin \theta \end{pmatrix}, \quad \frac{\partial(x,y)}{\partial(r,\theta)} = r, \quad dA = r \, dr \, d\theta
$$

**ב) גליליות**
$$
\begin{pmatrix} x \\ y \\ z \end{pmatrix} = \begin{pmatrix} \rho \cos \theta \\ \rho \sin \theta \\ z \end{pmatrix}, \quad \frac{\partial(x,y,z)}{\partial(\rho,\theta,z)} = \rho, \quad dV = \rho \, d\rho \, dz \, d\theta
$$

**ג) כדוריות**
$$
\begin{pmatrix} x \\ y \\ z \end{pmatrix} = \begin{pmatrix} r \sin \phi \cos \theta \\ r \sin \phi \sin \theta \\ r \cos \phi \end{pmatrix}, \quad \frac{\partial(x,y,z)}{\partial(r,\phi,\theta)} = r^2 \sin \phi, \quad dV = r^2 \sin \phi \, dr \, d\phi \, d\theta
$$
