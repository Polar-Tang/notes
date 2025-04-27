Bezier curve is an algorithm to create curves.
In a intersection we define a **lerp** (linear interpolation)
`lerp(P,Q,t)=(1−t)P+tQ`
Imagine we got a square with trhee lines, where very point is defined as p-${id} where id is the order of these point corresponding the clock's order
![[Pasted image 20250423143917.png]]
#### Linear Interpolation
A=lerp(P0, P1, t)
B=lerp(P1, P2, t)
C=lerp(P2, P3, t)
Where each point along the way is calculated by the previous lerp that came before it
![[Pasted image 20250423144339.png]]

#### Quadratic interpolation
D=lerp(A,B, t)
E=lerp(B,C, t)

####  Cubic interpolation

P=lerp(D,E,t )


#### **First Layer (Linear Interpolations):**

- **A** = lerp(P₀, P₁, t) = **(1 - t)P₀ + tP₁**
    
- **B** = lerp(P₁, P₂, t) = **(1 - t)P₁ + tP₂**
    
- **C** = lerp(P₂, P₃, t) = **(1 - t)P₂ + tP₃**
    

#### **Second Layer (Quadratic Interpolations):**

- **D** = lerp(A, B, t) = **(1 - t)A + tB**
    
- **E** = lerp(B, C, t) = **(1 - t)B + tC**
    

#### **Final Layer (Cubic Interpolation):**

- **P** = lerp(D, E, t) = **(1 - t)D + tE**

#### **Step 1: Expand D and E**

Substitute **A, B, C** into **D** and **E**:
![[Pasted image 20250423152347.png]]
#### **Step 2: Expand P (Final Point)**

Substitute **D** and **E** into **P**:
![[Pasted image 20250423152527.png]]
#### **Step 3: Combine Terms**

After expanding and simplifying:
![[Pasted image 20250423152549.png]]