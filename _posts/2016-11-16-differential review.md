---
layout: post
title:  "Differential review"
subtitle: "For the first mid-term exam of calculus"
date:   2016-11-16 12:34:01
categories: [lern-in-college]
---
  
## Functions
  
### Definition
	
```
	f: A to B, (describe domain)	
	A :	domain, B :	codomain
```

* All functions must image to one.
* 所有運算式必domain相同

### Composition of function:

```
	(f。g)(x) = f(g(x))
```
* No matter the order

```
	(f。g。h)(x) = ((f。g)。h)(x) = (f。(g。h))(x)
  =	f(g(h(x)))
```

* Be care for *Domain*

### Others

```
	Translation: 平移
		y = f(x)+-c
		y = f(x+-c)
	Reflection: 鏡射
		y = f(+-x)
		y = +-f(x)
	Symmetry: 對稱性
		f(-x) = f(x)	even function
		f(-x) = -f(x)	odd function
	Families of functions: parameter's (變數) chaning
		power functions
		polinomial functions: none-zero
		rational functions
	Monotonic (單調)
		increasing
		decreasing
	Asymptotes (漸近線)
		vertical
		horizontal
		oblique (斜)
	Cusps
		y = x^(2/3)
```

### Inverse

```
	f: A to B
	f^(-1)(x) = g(x)
	g: B to A
```

* Two functions must be one-to-one (injective) and subjective, namely bijective.
* Verticle line test for subjective.
* Horizontal line test for one-to-one.

```
	1. y = f(x),	A = ?,	B = ?
	2. Solve x as a function of x (if possible)
	3. x = f^(-1)(x)
```

### Limit

```
	lim	 f(x) = lim f(x) = L	<->		lim f(x) = L(finite number)
	x->a+		x->a-				x->a
```

* Local properties: A function has a jump at c, so it is not continues at c but exists limits close to c.
* infinite limit: increase/ decrease without bound, a behavior but not existance.
* +/* (次方)

```
	f(x) = p(x) / q(x)
	
	q(x) != 0 => lim f(x) = f(a)
				x->a
	
	q(x) == 0 and p(x) != 0 => DNE (vertical asymptote)
	
			  and p(x) == 0 => ?
```

* type 0/0
* lim(x->3) (x-3)/(x-3) 可約分，因x!=3，x-3!=0
* lim(x->infity) f(x) horizontal asymptote
* lim(x->infity) P(x) = lim(x->infity) CnX^n (highest term)

```
	L +- epsillon
	a +- delta	;	> or < N
```

* 用limit值求出f(x)+-epsillon之delta為何

```
	|f(x) - L| < epsillon if 0 < |x - a| < delta
 =>	|f(x) - L| < epsillon if 0 < |x - a| < delta1 < delta
```

### Continuity

```
	lim f(x) = f(c)
	x->c
	
	---------------
	
	[a,b]
	
	lim f(x) = f(c), x on [a, b]
	x->c
	
	lim f(x) = f(c)
	x->a+
	
	lim f(x) = f(c)
	x->b-
	
	---------------
	
	f and g continues at c
 =>	f +/* g continues at c
 
 	---------------
 	
 	lim g(x) = L and f(x) continues at L
 	x->c
 	
 	lim f(g(x)) = f( lim g(x) ) = f(L)
 	x->c			 x->c
 	
	---------------
	
	f(g(x)) and g(c) continue at c
 =>	lim f(g(x)) = f(g(c))
 	x->c
 	
	---------------
	
	f has f^-1
	if f is continuous then f^-1 is, too.
```

#### IVI - Intermediate-Value-Theorem

```
		f(x) continues at [a, b] and k is [f(a), f(b)]
	 =>	exists at least x in [a,b] that f(x) = k
```

```
	三角函數：c in natural domain -> they are continues
	
	x->0	sinx ~~ 0		cosx ~~ 1
```

* use lim f(g(x)) = f(lim g(x)) if f continues and limit g(x) exist

#### Squeeaing

```
	f(x) >= g(x) >= h(x) (all exist) and lim f(x) = lim h (x) = L
 =>	lim g(x) = L
```

## Derivative

```
	Differential is
	h = x - x0 , x = x0 + h
	mtan = lim (f(x0 + h) - f(x0))/h
		   h->0
	Domain(mtan) 包含於 Domain(f)
```

```
	limit < continuity < differentiability
```

```
	d(c * x^n)/dx = c * n * x^n-1	(c for linear)
	
	---------------
	
	d/dx [f(x)g(x)] = f'(x)g(x) + f(x)g'(x)
	
	---------------
	
	(sinx)' = cosx
	(cosx)' = -sinx
	(tanx)' = sec^2x
	(secx)' = tanx * secx (= 1/cosx)
	
	---------------
	
	Chain rule:
	d/dx [f(g(x))] = f'(g(x)) * g'(x)
```

```
	Implicit differentiation: y = f(x) explicit
	Related rates
	Local linear approximation:
		f(x) is differential
		(y - y0) ~~ dy = f'(x)dx
	differential form: dy = f'(x) * dx
	
	---------------
	
	f'(x)	[]
		increasing		>0
		decreasing		<0
		constant		=0
		***
		critical point: relative extrema at c
		f'(c) = 0	stationary point
		f'(c) DNE
		
	f''(x)	()
		concave up		>0
		concave down	<0
		***
		inflection points x0 that f''(x0) = 0
		可能的，還得看+-以及存在與否
	
	Local/Relative Extrema
		critical points
		+- f', f''
	
	Global/Absolute Extrema [a,b]
		critical points
		f(a), f(b), f(critical points)
		***
		Continues on [a,b]		-> exist on [a,b]
		and exist a relative extrema	-> the extrema is the absolute extrema
```
