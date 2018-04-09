# INS'hAck 2018: Math killer - hard -

**Category:** MISC |
**Points:** 60 |
**Description:**

> There's another way to solve the 'Math killer - easy' challenge, if you find it you'll get another flag!
>
> If you solve it the easy way first, its flag is a hint.
>
> Reminder: https://math-killer.ctf.insecurity-insa.fr/solve?a=...&b=...&c=...
>
> [CHALLENGE'S FILE](https://static.ctf.insecurity-insa.fr/chall.png)

## Write-up

Solving the easy one first give us a hint, the easy one flag was `INSA{try_positive_solutions_now}`  
So, we need to solve the eqation by using only positive numbers. After doing some research I found that [paper](http://ami.ektf.hu/uploads/papers/finalpdf/AMI_43_from29to41.pdf) by Bremner and MacLeod called **An unusual cubic representation problem**.
Also, I found a `CoCalc` code implementation by Alon Amit that solving similar equation.

I modified the code to be able to solve any a/(b+c)+b/(a+c)+c/(a+b)=N eqation with even N number.

```python

N=6

e=(4*(N^2)) + ((12*N)-3)

f=32*(N+3)

ee=EllipticCurve([0,e,0,f,0]) # Define the elliptic curve corresponding to the equation a/(b+c)+b/(a+c)+c/(a+b=N

ee.rank()

ee.gens()

P=ee(-200,680) # This is a generator for the group of rational points from ee.gens() result

def orig(P,N):
    x=P[0]
    y=P[1]
    a=(8*(N+3)-x+y)/(2*(N+3)*(4-x))
    b=(8*(N+3)-x-y)/(2*(N+3)*(4-x))
    c=(-4*(N+3)-(N+2)*x)/((N+3)*(4-x))
    da=denominator(a)
    db=denominator(b)
    dc=denominator(c)
    l=lcm(da,lcm(db,dc))
    return [a*l,b*l,c*l]

orig(P,N)

m=11 # The smallest integer m such that one of the points mP + T, T ∈ Tor(EN(Q))

u=orig(m*P,N) # Bremner and MacLeod noticed that 11P yields a positive solution for N=6

(a,b,c)=(u[0],u[1],u[2])

```

That give us a 134 digits a,b and c numbers which satisfy a given functional equation.

```python

...

(a,b,c)
(20260869859883222379931520298326390700152988332214525711323500132179943287700005601210288797153868533207131302477269470450828233936557,
 2250324022012683866886426461942494811141200084921223218461967377588564477616220767789632257358521952443049813799712386367623925971447,
 1218343242702905855792264237868803223073090298310121297526752830558323845503910071851999217959704024280699759290559009162035102974023)

a/(b+c)+b/(a+c)+c/(a+b)
6

```

By adding the numbers to the given [URL](https://math-killer.ctf.insecurity-insa.fr/solve?a=20260869859883222379931520298326390700152988332214525711323500132179943287700005601210288797153868533207131302477269470450828233936557&b=2250324022012683866886426461942494811141200084921223218461967377588564477616220767789632257358521952443049813799712386367623925971447&c=1218343242702905855792264237868803223073090298310121297526752830558323845503910071851999217959704024280699759290559009162035102974023) and BINGO!

```css
{
  "flags": {
    "easy": "INSA{try_positive_solutions_now}", 
    "hard": "INSA{OMG_you_actually_killed_math}"
  }, 
  "message": "Congrats for solving hard mode ! you get both flags for free :)", 
  "status": "success"
}
```

The flag is `INSA{OMG_you_actually_killed_math}`
