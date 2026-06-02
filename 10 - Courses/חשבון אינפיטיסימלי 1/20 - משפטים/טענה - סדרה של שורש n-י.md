---
course: אינפי 1
---
# טענה - סדרה של שורש n-י

**תנאים (אם...):**
יהיו
$$
(\sqrt[n]{a})_{n=1}^{\infty} ;\\\ a>0
$$

**מסקנה (אז...):**
הסדרה מתכנסת ומתקיים
$$
\lim_{n\rightarrow \infty}(\sqrt[n]{a}) = 1
$$

**אסטרטגיית הוכחה:**
- נפריד למקרים
- א' הסדרה קבועה על 1 וגבולה 1
- ב' אם $a>1$ אז
$$
\sqrt[n]{a}>1
$$
	- לכן נגדיר 
$$
(h_n)_{n=1}^{\infty} ;\\\ h_n=\sqrt[n]{a}-1
$$
	- לפי [[משפט - אי-שוויון ברנולי]] נקבל
$$a = (h_n+1)^n \ge 1+ nh_n \Rightarrow a - 1 \ge nh_n \Rightarrow h_n \le \frac{a-1}{n}$$
	- לפי [[משפט - הסנדוויץ']] הסדרה $(h_n)_{n=1}^{\infty}$ כלואה בין הסדרה הקבועה 0 לבין $c_n = \frac{a-1}{n}$ המתכנסות ל0 ולכן מתכנסת גם היא ל0  
- ג' אם $0<a<1$ אז
$$1<\frac{1}{a}$$
	- לכן לפי המקרה הקודם:
$$
\lim_{n\rightarrow\infty}\frac{1}{\sqrt[n]{{a}}} = \lim_{n\rightarrow\infty}\sqrt[n]\frac{1}{{a}} = 1 
$$
- לפיכך
$$\forall \epsilon>0 \\\ \exists N \\\ n>N \rightarrow |\frac{1}{\sqrt[n]{a}}-1| < \epsilon$$
	- מאחר ו $0<\sqrt[n]{a} <1$ 
$$
|\sqrt[n]{a} -1| < |\frac{\sqrt[n]{a} -1}{\sqrt[n]{a}}|= |1-\sqrt[n]{a}| < \epsilon  
$$
