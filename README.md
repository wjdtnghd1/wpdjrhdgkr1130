# 제어공학 chap 4 과제
2018732003 정수홍 

---  
## P4.8  
![image](https://github.com/juhwan98/Control-Engineering/assets/113814473/f2c875f1-2dc3-4240-93db-bdb20f97b30e)  
주변온도 하락은 $T_d(s)$에서 계단 감소로 표현된다.  
전자회로의 온도변화의 동역학은 다음과 같은 전달함수로 표현된다.  
$$G(s) = \frac{100}{(s^2 + 25s + 100)}$$
(a) $K$에 대한 시스템의 감도를 구하라.  
(b) 외란 $T_d(s)$가 출력 $Y(s)$에 미치는 영향을 구하라.  
(c) 출력 $Y(s)$가 크기가 $A$인 계단외란 입력[즉, $T_d(s)=A/s$]의 10%보다 작도록 하는 $K$의 범위를 찾아라.  

### sol  
(a)  

$$
\begin{aligned}
S^T _K &= \frac{1}{1+L(s)} \cdots L(s) = G_C(s) G(s)\\
&=\frac{1}{1 + \frac{K}{0.2s+1}\frac{100}{s^2+25s+100}}\\
&= \frac{(s^2+25s+100)(0.2s+1)}{(s^2+25s+100)(0.2s+1)+100K}
\end{aligned}
$$  

$\therefore S^T _K = \frac{(s^2+25s+100)(0.2s+1)}{(s^2+25s+100)(0.2s+1)+100K}$  

(b)  

$$
\begin{aligned}
Y(s)&=\frac{G(s)}{1+L}T_d(s)\\
&=\frac{(0.2s+1)\cdot 100}{(s^2+25s+100)\cdot(0.2s+1)+100K}\cdot T_d(s)
\end{aligned}
$$

$\therefore \frac{Y(s)}{T_d(s)} = \frac{(0.2s+1)\cdot 100}{(s^2+25s+100)\cdot(0.2s+1)+100K} $  

(c)  

$$
\begin{aligned}
\lim_{s\rightarrow 0} s \cdot Y(s) &= \frac{100A}{100+100K}\\
&= \frac{A}{1+K} \leq 0.1 \\
A-0.1\leq 0.1K
\end{aligned}
$$

$\therefore10A-1\leq K $

---
## P4.12  
![image](https://github.com/juhwan98/Control-Engineering/assets/113814473/a56c2e73-af3c-4750-8003-dbbb1728e1b6)   
(a) 각 시스템의 폐루프 전달함수 $T_1$와 $T_2$를 계산하라.  
(b) 공칭값이 $K_1 = K_2 = 1$일 떄 매개변수 $K_1$에 대한 두 시스템의 감도를 구하라.  

### sol  
(a)  

$$
\begin{aligned}
T_1(s) &= \frac{K_1/(s+4)\cdot K_2/(s-1)}{1+6\cdot K_1/(s+4)\cdot K_2/(s-1)}\\
&= \frac{K_1\cdot K_2}{s^2+3s+(6\cdot K_1\cdot K_2 -4)}\\
T_2(s) &= \frac{K_1/(s+4)\cdot K_2/(s-1)}{1-(2K_1/(s+4)-2K_2/(s-1))+ (-2K_1/(s+4)\cdot 2K_2/(s-1))}\\
&= \frac{K_1\cdot K_2}{s^2+(3-2K_1+2K_2)-4K_1K_2+2K_18K_2-4}
\end{aligned}
$$

$\therefore$  
$T_1(s) = \frac{K_1\cdot K_2}{s^2+3s+(6\cdot K_1\cdot K_2 -4)}$  
$T_2(s) = \frac{K_1\cdot K_2}{s^2+(3-2K_1+2K_2)-4K_1K_2+2K_1+8K_2-4}$  

(b)

감도 공식을 이용하여 문제를 풀이 가능  
$S^T_K = \frac{\partial T}{\partial K}\cdot \frac{K}{T}$  

```
%MATLAB
S_T1K1 = diff(T1,K1)*K1/T1;
S_T2K1 = diff(T2,K1)*K1/T2;

S_T1K1 = simplifyFraction(subs(S_T1K1,{K1 K2}, {1 1}))
S_T2K1 = simplifyFraction(subs(S_T2K1,{K1 K2}, {1 1}))
```  
% 출력      
![image](https://github.com/juhwan98/Control-Engineering/assets/113814473/921b99b3-a5cc-4968-91ac-b17cfb178c75)

$\therefore$  
$S^{T_1}{K_1} = \frac{s^2+3s-4}{s^2+3s+2}$  
$S^{T_2}{K_1} = \frac{s+4}{s+2}$  

---
## P4.17  
![image](https://github.com/juhwan98/Control-Engineering/assets/113814473/cbd58c1e-ff87-4526-aed5-159ce22f526f)  
 $K_m=30, R_f = 1 \Omega , K_f = K_i = 1, J = 0.1, b = 1$  
(a) $K=20$일 때 $\theta _d(t)$의 계단변화에 대한 시스템의 응답 $\theta (t)$를 결정하라.  
(b) $\theta _d(t) = 0$으로 가정하여 부하외란 $T_d(s) = A/s$의 영향을 구하라.  

(c) 입력 $r(t) = t, t > 0$일 때 정상상태 오차 $e_{ss}$를 결정하라. [ $T_d(s) = 0$ 으로 가정한다.]  

### sol
```
% MATLAB
syms clc;
syms s t R T_d A;

Km = 30;
Rf = 1;
Kf = 1;
Ki = 1;
J = 0.1;
b = 1;
K = 20;

G0 = Ki;
G1 = K*Km/Rf;
G2 = 1/(s*(J*s+b));
H = Kf;
Y = G0*G1*G2/(1 + G1*G2*H)*R + G2/(1 + G1*G2*H)*(-T_d);

% (a)
Y_1 = subs(Y, {R, T_d}, {1/s, 0});
Y_1 = ilaplace(Y_1)

$ (b)
Y_2 = subs(Y, {R, T_d}, {0, A/s});
Y_2 = limit(s*Y_2,s,0)

% (c)
E = R - Y;
E = subs(E, {R, T_d}, {1/s^2, 0});
ess = limit(s*E,s,0)
```
## 결과  

![image](https://github.com/juhwan98/Control-Engineering/assets/113814473/b20ec1be-de48-4268-93c4-6b497c18a257)
