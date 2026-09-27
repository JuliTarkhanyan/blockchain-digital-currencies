# Homework 3

## Find all integer solutions of the following equations

---

## 1. $18x + 30y = 126$

### Step 1 — Euclidean Algorithm

$$
30 = 1\cdot18 + 12
$$

$$
18 = 1\cdot12 + 6
$$

$$
12 = 2\cdot6 + 0
$$

Therefore:

$$
\gcd(18,30)=6
$$

Since:

$$
6\mid126
$$

integer solutions exist.

### Step 2 — Back-substitution

From:

$$
18=1\cdot12+6
$$

we get:

$$
6=18-12
$$

Since:

$$
12=30-18
$$

we have:

$$
6=18-(30-18)
$$

$$
6=2\cdot18-30
$$

Since:

$$
126=21\cdot6
$$

multiply by $21$:

$$
126=42\cdot18-21\cdot30
$$

Therefore, one particular solution is:

$$
x_0=42,\qquad y_0=-21
$$

### Step 3 — General solution

For

$$
ax+by=c
$$

the general solution is:

$$
x=x_0+\frac{b}{d}t
$$

$$
y=y_0-\frac{a}{d}t
$$

where $d=\gcd(a,b)$.

Therefore:

$$
x=42+\frac{30}{6}t=42+5t
$$

$$
y=-21-\frac{18}{6}t=-21-3t
$$

Choosing $t=-8$ gives the smaller particular solution $(2,3)$.

Thus:

$$
\boxed{x=2+5t,\qquad y=3-3t,\qquad t\in\mathbb Z}
$$

---

## 2. $35x + 22y = 1$

### Step 1 — Euclidean Algorithm

$$
35=1\cdot22+13
$$

$$
22=1\cdot13+9
$$

$$
13=1\cdot9+4
$$

$$
9=2\cdot4+1
$$

$$
4=4\cdot1+0
$$

Therefore:

$$
\gcd(35,22)=1
$$

Since:

$$
1\mid1
$$

integer solutions exist.

### Step 2 — Back-substitution

Starting from:

$$
1=9-2\cdot4
$$

Since:

$$
4=13-9
$$

we get:

$$
1=9-2(13-9)
$$

$$
1=3\cdot9-2\cdot13
$$

Since:

$$
9=22-13
$$

we get:

$$
1=3(22-13)-2\cdot13
$$

$$
1=3\cdot22-5\cdot13
$$

Since:

$$
13=35-22
$$

we get:

$$
1=3\cdot22-5(35-22)
$$

$$
1=8\cdot22-5\cdot35
$$

Therefore:

$$
1=-5\cdot35+8\cdot22
$$

One particular solution is:

$$
x_0=-5,\qquad y_0=8
$$

### Step 3 — General solution

Since $\gcd(35,22)=1$:

$$
x=-5+22t
$$

$$
y=8-35t
$$

Therefore:

$$
\boxed{x=-5+22t,\qquad y=8-35t,\qquad t\in\mathbb Z}
$$

---

## 3. $84x + 66y = 18$

### Step 1 — Euclidean Algorithm

$$
84=1\cdot66+18
$$

$$
66=3\cdot18+12
$$

$$
18=1\cdot12+6
$$

$$
12=2\cdot6+0
$$

Therefore:

$$
\gcd(84,66)=6
$$

Since:

$$
6\mid18
$$

integer solutions exist.

### Step 2 — Back-substitution

From:

$$
18=1\cdot12+6
$$

we get:

$$
6=18-12
$$

Since:

$$
12=66-3\cdot18
$$

we get:

$$
6=18-(66-3\cdot18)
$$

$$
6=4\cdot18-66
$$

Since:

$$
18=84-66
$$

we get:

$$
6=4(84-66)-66
$$

$$
6=4\cdot84-5\cdot66
$$

Multiply by $3$:

$$
18=12\cdot84-15\cdot66
$$

Therefore:

$$
x_0=12,\qquad y_0=-15
$$

### Step 3 — General solution

$$
x=12+\frac{66}{6}t
$$

$$
y=-15-\frac{84}{6}t
$$

Thus:

$$
x=12+11t
$$

$$
y=-15-14t
$$

Choosing $t=-1$ gives the smaller particular solution $(1,-1)$.

Therefore:

$$
\boxed{x=1+11t,\qquad y=-1-14t,\qquad t\in\mathbb Z}
$$

---

## 4. $6x + 10y + 15z = 41$

### Step 1 — Determine $x$

Reduce the equation modulo $5$:

$$
6x+10y+15z\equiv41\pmod5
$$

Therefore:

$$
6x\equiv41\pmod5
$$

Since:

$$
6\equiv1\pmod5
$$

and:

$$
41\equiv1\pmod5
$$

we get:

$$
x\equiv1\pmod5
$$

Therefore:

$$
x=1+5t
$$

where $t\in\mathbb Z$.

### Step 2 — Substitute

$$
6(1+5t)+10y+15z=41
$$

$$
6+30t+10y+15z=41
$$

$$
30t+10y+15z=35
$$

Divide by $5$:

$$
6t+2y+3z=7
$$

### Step 3 — Determine $z$

Reduce modulo $2$:

$$
3z\equiv7-6t\pmod2
$$

Since:

$$
3\equiv1,\qquad 7\equiv1,\qquad 6t\equiv0\pmod2
$$

we get:

$$
z\equiv1\pmod2
$$

Therefore:

$$
z=1+2s
$$

where $s\in\mathbb Z$.

### Step 4 — Determine $y$

Substitute:

$$
6t+2y+3(1+2s)=7
$$

$$
6t+2y+3+6s=7
$$

$$
2y=4-6t-6s
$$

Therefore:

$$
y=2-3t-3s
$$

### Final answer

$$
\boxed{
\begin{aligned}
x&=1+5t\\
y&=2-3t-3s\\
z&=1+2s
\end{aligned}
\qquad t,s\in\mathbb Z
}
$$

---

## 5. $12x + 15y + 20z = 7$

### Step 1 — Determine $z$

Reduce modulo $3$:

$$
12x+15y+20z\equiv7\pmod3
$$

Therefore:

$$
20z\equiv7\pmod3
$$

Since:

$$
20\equiv2,\qquad7\equiv1\pmod3
$$

we get:

$$
2z\equiv1\pmod3
$$

The inverse of $2$ modulo $3$ is $2$:

$$
z\equiv2\pmod3
$$

Therefore:

$$
z=2+3s
$$

### Step 2 — Substitute

$$
12x+15y+20(2+3s)=7
$$

$$
12x+15y+40+60s=7
$$

$$
12x+15y+60s=-33
$$

Divide by $3$:

$$
4x+5y+20s=-11
$$

### Step 3 — Determine $x$

Reduce modulo $5$:

$$
4x\equiv-11\pmod5
$$

Since:

$$
-11\equiv4\pmod5
$$

we get:

$$
4x\equiv4\pmod5
$$

Therefore:

$$
x\equiv1\pmod5
$$

Let:

$$
x=1+5t
$$

### Step 4 — Determine $y$

Substitute:

$$
4(1+5t)+5y+20s=-11
$$

$$
4+20t+5y+20s=-11
$$

$$
5y=-15-20t-20s
$$

Therefore:

$$
y=-3-4t-4s
$$

### Final answer

$$
\boxed{
\begin{aligned}
x&=1+5t\\
y&=-3-4t-4s\\
z&=2+3s
\end{aligned}
\qquad t,s\in\mathbb Z
}
$$

---

## 6. $14x + 21y + 9z = 5$

### Step 1 — Determine $x$

Reduce modulo $3$:

$$
14x+21y+9z\equiv5\pmod3
$$

Therefore:

$$
14x\equiv5\pmod3
$$

Since:

$$
14\equiv2,\qquad5\equiv2\pmod3
$$

we get:

$$
2x\equiv2\pmod3
$$

Therefore:

$$
x\equiv1\pmod3
$$

Let:

$$
x=1+3t
$$

### Step 2 — Substitute

$$
14(1+3t)+21y+9z=5
$$

$$
14+42t+21y+9z=5
$$

$$
42t+21y+9z=-9
$$

Divide by $3$:

$$
14t+7y+3z=-3
$$

### Step 3 — Determine $y$

Reduce modulo $3$:

$$
14t+7y\equiv-3\pmod3
$$

Therefore:

$$
2t+y\equiv0\pmod3
$$

So:

$$
y=-2t+3s
$$

### Step 4 — Determine $z$

Substitute:

$$
14t+7(-2t+3s)+3z=-3
$$

$$
14t-14t+21s+3z=-3
$$

$$
21s+3z=-3
$$

Divide by $3$:

$$
7s+z=-1
$$

Therefore:

$$
z=-1-7s
$$

### Final answer

$$
\boxed{
\begin{aligned}
x&=1+3t\\
y&=-2t+3s\\
z&=-1-7s
\end{aligned}
\qquad t,s\in\mathbb Z
}
$$

---

## 7. $6x + 10y + 15z + 21w = 17$

### Step 1 — Choose $x$ freely

Let:

$$
x=t
$$

Then:

$$
10y+15z+21w=17-6t
$$

### Step 2 — Determine $w$

Reduce modulo $5$:

$$
15z+21w\equiv17-6t\pmod5
$$

Since:

$$
15z\equiv0\pmod5
$$

we get:

$$
21w\equiv17-6t\pmod5
$$

Therefore:

$$
w\equiv2-t\pmod5
$$

Let:

$$
w=2-t+5r
$$

### Step 3 — Substitute

$$
10y+15z+21(2-t+5r)=17-6t
$$

Expand:

$$
10y+15z+42-21t+105r=17-6t
$$

Therefore:

$$
10y+15z=15t-25-105r
$$

Divide by $5$:

$$
2y+3z=3t-5-21r
$$

### Step 4 — Determine $z$

Reduce modulo $2$:

$$
3z\equiv3t-5-21r\pmod2
$$

Since:

$$
3\equiv1,\qquad-5\equiv1,\qquad-21\equiv1\pmod2
$$

we get:

$$
z\equiv t-1-r\pmod2
$$

Therefore:

$$
z=t-1-r+2s
$$

### Step 5 — Determine $y$

Substitute:

$$
2y+3(t-1-r+2s)=3t-5-21r
$$

Expand:

$$
2y+3t-3-3r+6s=3t-5-21r
$$

Therefore:

$$
2y=-2-18r-6s
$$

Hence:

$$
y=-1-9r-3s
$$

### Final answer

$$
\boxed{
\begin{aligned}
x&=t\\
y&=-1-3s-9r\\
z&=t-1+2s-r\\
w&=2-t+5r
\end{aligned}
\qquad t,s,r\in\mathbb Z
}
$$

---

## 8. $12x + 18y + 25z + 35w = 11$

### Step 1 — Choose $x$ freely

Let:

$$
x=t
$$

Then:

$$
18y+25z+35w=11-12t
$$

### Step 2 — Determine $y$

Reduce modulo $5$:

$$
18y\equiv11-12t\pmod5
$$

Since:

$$
18\equiv3,\qquad11\equiv1,\qquad12\equiv2\pmod5
$$

we get:

$$
3y\equiv1-2t\pmod5
$$

The inverse of $3$ modulo $5$ is $2$:

$$
y\equiv2(1-2t)\pmod5
$$

Therefore:

$$
y\equiv2-4t\pmod5
$$

Since:

$$
-4\equiv1\pmod5
$$

we can write:

$$
y=t+2+5s
$$

### Step 3 — Substitute

$$
18(t+2+5s)+25z+35w=11-12t
$$

Expand:

$$
18t+36+90s+25z+35w=11-12t
$$

Therefore:

$$
30t+90s+25z+35w=-25
$$

Divide by $5$:

$$
6t+18s+5z+7w=-5
$$

### Step 4 — Determine $w$

Reduce modulo $5$:

$$
7w\equiv-5-6t-18s\pmod5
$$

Therefore:

$$
2w\equiv-t+2s\pmod5
$$

The inverse of $2$ modulo $5$ is $3$:

$$
w\equiv3(-t+2s)\pmod5
$$

Thus:

$$
w\equiv-3t+6s\pmod5
$$

Since:

$$
-3\equiv2,\qquad6\equiv1\pmod5
$$

we get:

$$
w\equiv2t+s\pmod5
$$

Therefore:

$$
w=2t+s+5r
$$

### Step 5 — Determine $z$

Substitute:

$$
6t+18s+5z+7(2t+s+5r)=-5
$$

Expand:

$$
6t+18s+5z+14t+7s+35r=-5
$$

Therefore:

$$
20t+25s+5z+35r=-5
$$

Divide by $5$:

$$
4t+5s+z+7r=-1
$$

Therefore:

$$
z=-1-4t-5s-7r
$$

### Final answer

$$
\boxed{
\begin{aligned}
x&=t\\
y&=t+2+5s\\
z&=-1-4t-5s-7r\\
w&=2t+s+5r
\end{aligned}
\qquad t,s,r\in\mathbb Z
}
$$

---

# Final Answers — Summary

| # | General integer solution |
|---|---|
| **1** | $x=2+5t,\quad y=3-3t$ |
| **2** | $x=-5+22t,\quad y=8-35t$ |
| **3** | $x=1+11t,\quad y=-1-14t$ |
| **4** | $x=1+5t,\quad y=2-3t-3s,\quad z=1+2s$ |
| **5** | $x=1+5t,\quad y=-3-4t-4s,\quad z=2+3s$ |
| **6** | $x=1+3t,\quad y=-2t+3s,\quad z=-1-7s$ |
| **7** | $x=t,\quad y=-1-3s-9r,\quad z=t-1+2s-r,\quad w=2-t+5r$ |
| **8** | $x=t,\quad y=t+2+5s,\quad z=-1-4t-5s-7r,\quad w=2t+s+5r$ |

where all parameters satisfy:

$$
t,s,r\in\mathbb Z
$$
