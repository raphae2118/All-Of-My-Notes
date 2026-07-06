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

