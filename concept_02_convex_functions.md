# Concept 2: Convex vs Non-Convex Functions

---

## Real Life Story

Imagine two landscapes:

A SMOOTH BOWL (Convex): You are inside a smooth cereal bowl. No matter where you start, if you keep walking downhill, you will ALWAYS reach the same lowest point at the bottom center. There is only ONE valley.

A MOUNTAIN RANGE (Non-Convex): You are hiking in the Himalayas. There are many valleys and peaks. If you walk downhill, you might get stuck in a small valley that is NOT the deepest valley. Gradient Descent can get stuck here!

Why this matters for your assignment: LASSO is CONVEX, so gradient descent (and variants) will always find the true minimum. No fear of getting stuck!

---

## Images

![Convex vs Non-Convex comparison](C:/Users/kush3/.gemini/antigravity/brain/5f339117-f2bc-4f99-9a8f-2ae4646e76b9/convex_vs_nonconvex_1773085951275.png)

![3D convex bowl visualization](C:/Users/kush3/.gemini/antigravity/brain/5f339117-f2bc-4f99-9a8f-2ae4646e76b9/convex_set_bowl_1773085966025.png)

---

## Math: The Line-Segment Test (Definition of Convexity)

```
A function f is CONVEX if for any two points x, y
and any t in the range [0, 1]:

   f(t*x + (1-t)*y)  <=  t*f(x) + (1-t)*f(y)

In English:
Pick ANY two points on the curve.
Draw a straight line between them.
The curve must be BELOW (or touching) that line.

t = how far along the line you are (0 = at y, 1 = at x)
Left side  = actual function value (the curve)
Right side = the line between f(x) and f(y)
```

---

## Deep Dive: What is t and Why 0 <= t <= 1?

Think of t as a SLIDER between two points.

Analogy: Imagine a straight road between your Home (point y) and your School (point x). The variable t tells you how far along the road you are:

- t = 0  -->  You are at Home (y)
- t = 1  -->  You are at School (x)
- t = 0.5  -->  You are exactly in the middle
- t = 0.2  -->  You are 20% of the way from Home toward School
- t = 0.8  -->  You are 80% of the way (almost at School)

Mathematically, t*x + (1-t)*y creates a weighted average (a convex combination):

```
point_on_line = t * x + (1-t) * y

When t = 0:   0*x + 1*y = y          (at point y)
When t = 0.2: 0.2*x + 0.8*y         (20% toward x)
When t = 0.5: 0.5*x + 0.5*y         (midpoint, equal mix)
When t = 0.8: 0.8*x + 0.2*y         (80% toward x)
When t = 1:   1*x + 0*y = x          (at point x)

t and (1-t) always add up to 1!
That is why it is a WEIGHTED AVERAGE - weights must sum to 1.

WHY 0 <= t <= 1?
If t < 0 or t > 1, we would go BEYOND the two points.
We only care about the line BETWEEN x and y.
```

Why we used t = 0.5 in earlier examples: t = 0.5 is the midpoint, which is the simplest to calculate. But for convexity, the inequality must hold for EVERY t in [0, 1], not just 0.5! Let us verify with DIFFERENT t values:

---

## Visual: See t on the Graph of f(x) = x squared

![Graph showing parameter t on f(x)=x squared](C:/Users/kush3/.gemini/antigravity/brain/5f339117-f2bc-4f99-9a8f-2ae4646e76b9/t_parameter_clean_1773088804010.png)

How to read this graph step by step:

```
We pick two points on the parabola:
  Point A: x=1, f(1)=1    (left point)
  Point B: x=3, f(3)=9    (right point)

The RED line connects A to B (straight line).
The BLUE curve is f(x) = x squared (the parabola).

Now, t slides us along the x-axis between x=1 and x=3:

At each t, we look at TWO heights:
  GREEN dot = curve height = f(t*1 + (1-t)*3)    (LEFT side of inequality)
  RED dot   = line height  = t*f(1) + (1-t)*f(3) (RIGHT side of inequality)

CONVEX means: GREEN is ALWAYS below RED
(curve is always below the straight line)

t=0.2  -->  x=2.6:  curve=6.76, line=7.4   (gap=0.64)
t=0.5  -->  x=2.0:  curve=4.0,  line=5.0   (gap=1.0)
t=0.8  -->  x=1.4:  curve=1.96, line=2.6   (gap=0.64)

The gap between red and green = how much the curve
DIPS BELOW the line. For convex functions, this gap
is ALWAYS >= 0!
```

---

## Checking f(x) = x squared with MULTIPLE t values (x=1, y=3)

t = 0.5 (midpoint):
```
Left:   f(0.5*1 + 0.5*3) = f(2) = 4
Right:  0.5*1 + 0.5*9 = 5
Is 4 <= 5? YES
```

t = 0.2 (closer to y=3):
```
Left:   f(0.2*1 + 0.8*3) = f(2.6) = 6.76
Right:  0.2*f(1) + 0.8*f(3) = 0.2 + 7.2 = 7.4
Is 6.76 <= 7.4? YES
```

t = 0.8 (closer to x=1):
```
Left:   f(0.8*1 + 0.2*3) = f(1.4) = 1.96
Right:  0.8*f(1) + 0.2*f(3) = 0.8 + 1.8 = 2.6
Is 1.96 <= 2.6? YES
```

t = 0.1 (very close to y=3):
```
Left:   f(0.1*1 + 0.9*3) = f(2.8) = 7.84
Right:  0.1*1 + 0.9*9 = 8.2
Is 7.84 <= 8.2? YES
```

Pattern: No matter what t we pick, curve value (left) is ALWAYS <= line value (right). That is convexity!

---

## sin(x) FAILS - proof it is NOT convex (x=0, y=pi)

t = 0.5:
```
Left:   f(0.5*0 + 0.5*pi) = sin(pi/2) = 1
Right:  0.5*sin(0) + 0.5*sin(pi) = 0 + 0 = 0
Is 1 <= 0? NO! Curve goes ABOVE the line!
```

---

## THREE Ways to Check if a Function is Convex

There are 3 mathematical tests, each progressively easier to use:

---

### Test 1: Line-Segment Test (already shown above)

```
For ALL x, y and ALL t in [0, 1]:

   f(t*x + (1-t)*y)  <=  t*f(x) + (1-t)*f(y)

"The function value at any weighted average of two points
 is <= the weighted average of the function values"
```

### Test 2: First-Order Condition (using the gradient)

```
If f is differentiable, then f is convex if and only if:

   f(y) >= f(x) + f'(x) * (y - x)     for ALL x, y

In English:
"The function always lies ABOVE its tangent line"

Picture it: Draw a tangent line at any point on the curve.
The entire curve must sit ABOVE that tangent line.
```

### Test 3: Second-Order Condition (using the second derivative) - EASIEST!

```
If f is twice differentiable, then f is convex if and only if:

   f''(x) >= 0     for ALL x       (1D case)
   Hessian is PSD  for ALL x       (multi-D case, explained below)

In English:
"The second derivative is never negative"
= "The curve always bends UPWARD (or is straight)"

f''(x)  = second derivative (how the slope itself changes)
>= 0    = the slope is always increasing (or constant)

This is the QUICKEST test! Just take the second derivative
and check if it is always >= 0.
```

---

## How does f''(x) tell us bending direction?

Think of it like driving a car:
- f(x) = your position (where you are)
- f'(x) = your speed (how fast position changes)
- f''(x) = your acceleration (how fast speed changes)

Now apply this to a curve:
- f'(x) = the slope of the curve at point x
- f''(x) = how the slope itself is changing

Three cases:

| f''(x) | What it means | Shape | Example |
|--------|--------------|-------|---------|
| f''(x) > 0 | Slope is increasing (going from negative to zero to positive) | Bends UPWARD like a bowl | f(x) = x squared, f''=2 > 0 |
| f''(x) < 0 | Slope is decreasing (going from positive to zero to negative) | Bends DOWNWARD like a hill | f(x) = -x squared, f''=-2 < 0 |
| f''(x) = 0 | Slope is constant | Straight line (flat) | f(x) = 3x+1, f''=0 |

Numerical proof for f(x) = x squared:
```
At x=-2: f'(-2) = -4   (slope goes down-left)
At x=-1: f'(-1) = -2   (slope is less steep)
At x= 0: f'(0)  =  0   (flat, bottom of bowl!)
At x= 1: f'(1)  = +2   (slope goes up-right)
At x= 2: f'(2)  = +4   (steeper upward)

The slope goes: -4, -2, 0, +2, +4
It is INCREASING! That is f''(x) = 2 > 0 = bends UPWARD
```

So: f''(x) >= 0 everywhere = always bends upward = CONVEX!

---

## What is the HESSIAN MATRIX? (Multi-D second derivative)

In 1D, f''(x) is just one number. But what if our function has 2 or more variables (like f(x1, x2))? A single number cannot capture the curvature in all directions! We need a matrix of all second derivatives, which is called the Hessian.

The Hessian matrix is a table of ALL second derivatives:

```
For a function f(x1, x2) with 2 variables:

H = | d2f/dx1^2      d2f/dx1*dx2 |
    | d2f/dx2*dx1    d2f/dx2^2   |

In English (all 4 entries):
  Row 1, Col 1 (d2f/dx1^2):      Curvature along x1 direction
  Row 1, Col 2 (d2f/dx1*dx2):    How x1-slope changes when
                                   you move in x2 (cross-effect)
  Row 2, Col 1 (d2f/dx2*dx1):    How x2-slope changes when
                                   you move in x1 (cross-effect)
  Row 2, Col 2 (d2f/dx2^2):      Curvature along x2 direction

The Hessian is SYMMETRIC: d2f/dx1*dx2 = d2f/dx2*dx1
```

## The Hessian as a Transformation: What does H * v mean?

You are right! A matrix is a linear transformation. So what does the Hessian transform?

```
INPUT:  a direction vector v = "which direction do I walk?"
OUTPUT: H * v = "how does the GRADIENT change when I walk in that direction"
```

Analogy: Imagine you have a GPS that shows the slope of the ground. You are standing on a hilly surface.

```
You ask: "If I take one step EAST, how will the GPS readings change?"
The Hessian answers: "The slope in east direction will change by 6,
                       and the slope in north direction will change by 2"

That answer IS the Hessian times your direction vector!
```

Numerical example with H = [[6, 2], [2, 4]]:

```
Question 1: "What happens to the gradient if I walk in direction [1, 0]?" (pure east)

  H * [1, 0] = [6*1 + 2*0,  2*1 + 4*0] = [6, 2]

  Answer: The gradient changes by [6, 2].
  The east-slope increases by 6 (steep curvature in this direction)
  The north-slope increases by 2 (cross-effect)

Question 2: "What happens if I walk in direction [0, 1]?" (pure north)

  H * [0, 1] = [6*0 + 2*1,  2*0 + 4*1] = [2, 4]

  Answer: The gradient changes by [2, 4].
  The east-slope increases by 2 (cross-effect)
  The north-slope increases by 4 (curvature in north direction)

Question 3: "What happens if I walk in the EIGENVECTOR direction [0.82, 0.57]?"

  H * [0.82, 0.57] = [6*0.82 + 2*0.57,  2*0.82 + 4*0.57]
                    = [4.92 + 1.14,  1.64 + 2.28]
                    = [6.06, 3.92]

  But 7.24 * [0.82, 0.57] = [5.94, 4.13]  (approximately the same!)

  This is the magic of eigenvectors:
  H * eigenvector = eigenvalue * eigenvector
  The Hessian just SCALES the eigenvector, does not rotate it!
  The eigenvalue (7.24) tells you HOW MUCH it scales.
```

## Visual: Eigenvector vs Normal Direction

![Hessian transforms normal vectors by rotating and scaling, but eigenvectors only get scaled](C:/Users/kush3/.gemini/antigravity/brain/5f339117-f2bc-4f99-9a8f-2ae4646e76b9/hessian_transformation_1773142985428.png)

Here is the key difference:

```
NORMAL DIRECTION like [1, 0] (east):
  You walk EAST          -->  input arrow points right
  H * [1, 0] = [6, 2]   -->  output arrow points UP-RIGHT
  
  The output points in a DIFFERENT direction than the input!
  The Hessian ROTATED your vector (changed its direction).
  "I walked east, but the gradient changed in a tilted direction"

EIGENVECTOR DIRECTION like [0.82, 0.57]:
  You walk tilted-right  -->  input arrow points tilted-right
  H * [0.82, 0.57] = [5.94, 4.13]  -->  output ALSO tilted-right!
  
  The output points in the SAME direction as the input!
  The Hessian only STRETCHED your vector (made it longer), did NOT rotate.
  "I walked tilted-right, gradient changed in the SAME tilted-right direction"
  The stretch factor = eigenvalue = 7.24
```

Why are eigenvectors special?

```
Walk in a NORMAL direction:
  The gradient changes in a MIXED way (partly along your direction,
  partly sideways). Confusing! Hard to interpret.

Walk in an EIGENVECTOR direction:
  The gradient changes PURELY along your direction.
  No sideways component. Clean and simple!
  The eigenvalue tells you exactly how much.

That is why eigenvectors are the "natural axes" of the surface.
The surface is simplest to understand along these directions.
```

Summary of what the Hessian transformation means:

```
Regular matrix:   transforms a vector (rotates + scales it)
Hessian matrix:   transforms a DIRECTION into GRADIENT-CHANGE

  Input:  "I want to walk in direction v"
  Output: "Here is how the slope will change: H*v"

  If H*v points the SAME way as v   -->  surface curves UP in that direction
  If H*v points OPPOSITE to v       -->  surface curves DOWN (saddle!)
  If H*v = 0                        -->  surface is FLAT in that direction
```

---

## What does Positive Semi-Definite (PSD) mean?

```
A matrix H is Positive Semi-Definite (PSD) if:

   ALL its eigenvalues are >= 0

In English:
"The surface curves UPWARD (or is flat) in EVERY direction"

  All eigenvalues > 0  -->  strictly convex (perfect bowl)
  Some eigenvalues = 0 -->  convex but flat in some directions
  Any eigenvalue < 0   -->  NOT convex (surface dips down)
```

---

## Visual: What Eigenvalues Look Like as 3D Shapes

![Eigenvalues determine the shape: Bowl, Saddle, Trough](C:/Users/kush3/.gemini/antigravity/brain/5f339117-f2bc-4f99-9a8f-2ae4646e76b9/eigenvalues_psd_visual_1773089670069.png)

---

## Intuitive Explanation: What ARE Eigenvalues?

Think of a rubber sheet stretched over a frame. You push down in the center to make a bowl shape. Now the bowl might be steeper on one side and gentler on the other.

Two separate things to understand:

```
EIGENVECTOR = a DIRECTION (shown as ARROW in the image)
   "In WHICH direction does the surface curve?"
   It is a direction like "east-west" or "north-south"
   Shown as an ARROW because it is a direction

EIGENVALUE = a NUMBER (shown as LABEL on the arrow)
   "HOW MUCH does the surface curve in that direction?"
   It is just a number like 5 or 2 or -3
   Bigger number = MORE curvature in that direction

TOGETHER: "The surface curves by amount lambda in direction v"
```

What does "steep" vs "gentle" curvature mean?

Imagine standing at the bottom of a bowl and walking in two directions:

Direction 1 (eigenvalue = 7.24, called "steep"):
Walk 1 step right and the ground rises A LOT. You quickly climb uphill.
Like walking up a steep ramp. The surface shoots up fast.

Direction 2 (eigenvalue = 2.76, called "gentle"):
Walk 1 step forward and the ground rises only a little. Gradual slope.
Like walking on a gentle hill. The surface rises slowly.

```
Walk 1 unit in direction 1: height goes up by about 3.62
Walk 1 unit in direction 2: height goes up by about 1.38
```

Same distance walked, but direction 1 takes you 2.6x higher!
That is what "steeper curvature" means: the surface rises faster.

Big eigenvalue = steep sides (surface rises fast)
Small eigenvalue = gentle sides (surface rises slowly)

---

## Example with H = [[6, 2], [2, 4]]

```
Eigenvalue L1 = 7.24  with  Eigenvector v1 = [0.82, 0.57]
Eigenvalue L2 = 2.76  with  Eigenvector v2 = [-0.57, 0.82]

What this means:
  In the direction [0.82, 0.57] (tilted right): curvature = 7.24 (STEEP)
  In the direction [-0.57, 0.82] (tilted left):  curvature = 2.76 (gentle)

The bowl is STEEPER in direction v1 and GENTLER in direction v2.
Both are positive --> Bowl curves UP in BOTH directions --> CONVEX!
```

So in the image:
- The arrows show eigenvectors = which 2 directions the surface curves along
- The numbers (L1, L2) show eigenvalues = how steeply it curves in each direction
- A longer arrow was drawn for the bigger eigenvalue to show "more curvature here"

---

## Three real-world shapes based on eigenvalues

| Case | Eigenvalues | Shape | What it looks like | Convex? |
|------|------------|-------|-------------------|---------|
| Case 1 | L1=5, L2=2 (both positive) | BOWL | Cereal bowl, curving up on all sides | YES, CONVEX |
| Case 2 | L1=3, L2=-2 (one negative!) | SADDLE | Pringles chip, up in one direction, down in another | NO, NOT convex |
| Case 3 | L1=4, L2=0 (one is zero) | TROUGH | Rain gutter, curves up sideways, flat along the length | YES, barely convex |

Summary:
- ALL eigenvalues positive --> Bowl shape --> Convex (GD will find the minimum!)
- ANY eigenvalue negative --> Saddle shape --> Not convex (GD gets confused)
- Eigenvalue zero --> Trough shape --> Still convex, but has a flat direction

---

## Multi-D Example 1: f(x1, x2) = x1 squared + x2 squared (circular bowl)

```
Step 1: Compute partial derivatives
  df/dx1 = 2*x1         df/dx2 = 2*x2

Step 2: Compute second partial derivatives
  d2f/dx1^2     = 2       d2f/dx1*dx2 = 0
  d2f/dx2*dx1   = 0       d2f/dx2^2   = 2

Step 3: Write the Hessian matrix
  H = | 2  0 |
      | 0  2 |

Step 4: Find eigenvalues
  det(H - lambda*I) = 0
  (2-lambda)(2-lambda) = 0  -->  L1 = 2, L2 = 2

Step 5: Check PSD
  Both eigenvalues = 2 > 0  -->  PSD  -->  CONVEX!
```

Both eigenvalues are positive, so the surface curves upward in ALL directions. It is a nice round bowl. CONVEX!

---

## Multi-D Example 2: f(x1, x2) = 3*x1 squared + 2*x1*x2 + 2*x2 squared (elliptical bowl)

```
Step 1: Compute partial derivatives
  df/dx1 = 6*x1 + 2*x2       df/dx2 = 2*x1 + 4*x2

Step 2: Compute second partial derivatives
  d2f/dx1^2     = 6       d2f/dx1*dx2 = 2
  d2f/dx2*dx1   = 2       d2f/dx2^2   = 4

Step 3: Write the Hessian matrix
  H = | 6  2 |
      | 2  4 |

Step 4: Find eigenvalues
  det(H - lambda*I) = (6-L)(4-L) - 4 = L^2 - 10L + 20 = 0
  L = (10 +/- sqrt(100-80))/2 = (10 +/- sqrt(20))/2
  L1 = (10 + 4.47)/2 = 7.24
  L2 = (10 - 4.47)/2 = 2.76

Step 5: Check PSD
  L1 = 7.24 > 0
  L2 = 2.76 > 0
  Both positive --> PSD --> CONVEX!
```

Both eigenvalues positive means elliptical bowl. CONVEX!

Note: The ratio L1/L2 = 7.24/2.76 = 2.6 is the condition number. The bigger this ratio, the more elongated the bowl, and the more GD zigzags (you will see this in Part 3 of your assignment when condition number > 100!)

---

## Multi-D Example 3: f(x1, x2) = x1 squared - x2 squared (saddle shape)

```
  H = |  2   0 |
      |  0  -2 |

  Eigenvalues: L1 = 2, L2 = -2
  L2 < 0  -->  NOT positive semi-definite --> NOT CONVEX!
```

One negative eigenvalue means the surface curves DOWNWARD in the x2 direction (like a horse saddle). NOT convex!

---

## Worked Examples: Proving Each 1D Function

---

### Example 1: f(x) = x squared - CONVEX

Method: Second-Order Test (easiest)
```
f(x)   = x^2
f'(x)  = 2x       (first derivative)
f''(x) = 2        (second derivative)

Is f''(x) >= 0?  -->  2 >= 0  -->  YES, always!
```
f''(x) = 2 is always positive, so f(x) = x squared is CONVEX.

Verify with Line-Segment Test (x=1, y=3, t=0.5):
```
Left:   f(0.5*1 + 0.5*3) = f(2) = 4
Right:  0.5*f(1) + 0.5*f(3) = 0.5*1 + 0.5*9 = 5
Is 4 <= 5?  YES!
```

---

### Example 2: f(x) = |x| - CONVEX (even with the corner!)

Method: Line-Segment Test (cannot use f'' because |x| has no derivative at x=0)
```
We need to show: |t*a + (1-t)*b| <= t*|a| + (1-t)*|b|
This is actually the famous TRIANGLE INEQUALITY!
It is always true.
```

Verify with numbers (x = -2, y = 4, t = 0.5):
```
Left:   f(0.5*(-2) + 0.5*4) = f(-1 + 2) = f(1) = |1| = 1
Right:  0.5*|-2| + 0.5*|4| = 0.5*2 + 0.5*4 = 1 + 2 = 3
Is 1 <= 3?  YES!
```
|x| is convex! The "corner" at x=0 does not break convexity.
(Corners just mean the function is not differentiable there, but it is still convex)

---

### Example 3: f(x) = e to the power x - CONVEX

Method: Second-Order Test
```
f(x)   = e^x
f'(x)  = e^x        (derivative of e^x is e^x)
f''(x) = e^x        (second derivative is also e^x)

Is f''(x) >= 0?  -->  e^x > 0 for ALL x  -->  YES!
(e^x is always positive: e^0=1, e^1=2.7, e^(-1)=0.37... never zero or negative)
```
f''(x) = e^x > 0 always, so f(x) = e^x is CONVEX.

---

### Example 4: f(x) = sin(x) - NOT CONVEX

Method: Second-Order Test
```
f(x)   = sin(x)
f'(x)  = cos(x)
f''(x) = -sin(x)

Is f''(x) >= 0 always?
At x = pi/2:   f''(pi/2) = -sin(pi/2) = -1  -->  NEGATIVE!
At x = 3*pi/2: f''(3*pi/2) = -sin(3*pi/2) = +1 --> positive
```
f''(x) = -sin(x) is sometimes negative, sometimes positive.
sin(x) is NOT CONVEX.
(It has infinitely many hills and valleys, gradient descent would get stuck!)

---

### Example 5: f(x) = x to the 4th minus x squared - NOT CONVEX

Method: Second-Order Test
```
f(x)   = x^4 - x^2
f'(x)  = 4x^3 - 2x
f''(x) = 12x^2 - 2

Is f''(x) >= 0 always?
At x = 0:  f''(0) = 12*0 - 2 = -2  -->  NEGATIVE!
At x = 1:  f''(1) = 12*1 - 2 = 10  -->  positive
```
f''(0) = -2 < 0, so the curve bends DOWNWARD near x=0.
f(x) = x^4 - x^2 is NOT CONVEX.
(It has two dips: one near x = -0.7 and one near x = +0.7)

---

## Important Convexity Rules (used in your assignment!)

```
RULE 1: f convex, g convex  -->  f + g is CONVEX
        (sum of convex functions is convex)

RULE 2: f convex, c >= 0    -->  c*f is CONVEX
        (scaling a convex function keeps it convex)

RULE 3: f(Ax + b) is convex if f is convex
        (affine transformation preserves convexity)
```

---

## Why LASSO is Convex (using the rules above)

```
LASSO:  f(B) = (1/2N) * SUM(yi - Xi*B)^2  +  lambda * SUM|Bj|
               ________________________/     ____________/
                Part A: Squared                Part B: L1
                error                          penalty

Part A: (yi - Xi*B)^2 = squared function of a linear function
  x^2 is convex (f''=2 >= 0)
  (y-X*B) is linear in B --> Rule 3 applies
  Sum of convex = convex --> Rule 1

Part B: |Bj| is convex (triangle inequality)
  lambda >= 0, so lambda*|Bj| is convex --> Rule 2
  Sum of convex = convex --> Rule 1

Part A convex + Part B convex = LASSO is CONVEX!
All our algorithms WILL find the true minimum!
```

---

## Hand-Worked Example

Let us verify f(x) = x squared is convex using the definition:

Pick two points: x = 1, y = 3, and t = 0.5 (midpoint)

Left side (function at the midpoint):
```
t*x + (1-t)*y = 0.5*1 + 0.5*3 = 2
f(2) = 2^2 = 4
```

Right side (line between f(x) and f(y)):
```
t*f(x) + (1-t)*f(y) = 0.5*f(1) + 0.5*f(3)
= 0.5*1 + 0.5*9
= 0.5 + 4.5 = 5
```

Check: Is 4 <= 5? YES! The curve value (4) is below the line value (5), so f(x) = x squared is convex!

---

## Common Mistakes

| # | Wrong | Correct |
|---|-------|---------|
| 1 | "Convex means it curves upward" (too vague!) | Convex means the line-segment test passes for ALL pairs of points |
| 2 | Confusing convex function with convex set | Function = the curve shape. Set = a region (we learn this in Concept 3!) |
| 3 | Thinking |x| is not convex because it has a "corner" | |x| IS convex! Corners are fine. It still has ONE minimum and passes the line test |

---

## Mini Quiz

Q1 (Conceptual): Why is it important for your assignment that LASSO is convex? What guarantee does it give us?

Q2 (Numerical): Check if f(x) = |x| is convex at points x = -2, y = 4, t = 0.5. (Hint: compute both sides of the inequality)

Q3 (Visual): If you see a loss curve during training that oscillates up and down instead of smoothly decreasing, is the function likely convex or non-convex? Why?
