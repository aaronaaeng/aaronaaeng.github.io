

```python
# That numpy tho
import numpy as np
```


```python
# probability that the disc moves right, r
r = 1/2
```


```python
#width of the plinko
n = 9
```


```python
# make that transition  (long row to short row)
Q = np.zeros([n,n])
for i in range(1,n-1):
    Q[i,i] = 2*r*(1-r)
    Q[i,i+1] = r**2
    Q[i,i-1] = (1-r)**2
Q[0,0] = 1-r
Q[0,1] = r
Q[-1,-1] = r
Q[-1,-2] = 1-r
if n <=10:
    print(Q)
```

    [[ 0.5   0.5   0.    0.    0.    0.    0.    0.    0.  ]
     [ 0.25  0.5   0.25  0.    0.    0.    0.    0.    0.  ]
     [ 0.    0.25  0.5   0.25  0.    0.    0.    0.    0.  ]
     [ 0.    0.    0.25  0.5   0.25  0.    0.    0.    0.  ]
     [ 0.    0.    0.    0.25  0.5   0.25  0.    0.    0.  ]
     [ 0.    0.    0.    0.    0.25  0.5   0.25  0.    0.  ]
     [ 0.    0.    0.    0.    0.    0.25  0.5   0.25  0.  ]
     [ 0.    0.    0.    0.    0.    0.    0.25  0.5   0.25]
     [ 0.    0.    0.    0.    0.    0.    0.    0.5   0.5 ]]
    


```python
# evals (w) and evecs (v)
w,v = np.linalg.eig(np.transpose(Q))
# grab the eigenvector corresponding to the largest eigenvalue (which is 1)
steadystate = v[:,w==np.max(w)]
# but if we want this eigenvector as a probability, we need to normalize it
steadystate = steadystate/steadystate.sum()
print(steadystate)
```

    [[ 0.0625]
     [ 0.125 ]
     [ 0.125 ]
     [ 0.125 ]
     [ 0.125 ]
     [ 0.125 ]
     [ 0.125 ]
     [ 0.125 ]
     [ 0.0625]]
    


```python
# make that transition  (short row to short row)
P = np.zeros([n,n])
for i in range(1,n-1):
    P[i,i] = 2*r*(1-r)
    P[i,i+1] = r**2
    P[i,i-1] = (1-r)**2
P[0,0] = 1-r**2
P[0,1] = r**2
P[-1,-1] = 1-(1-r)**2
P[-1,-2] = (1-r)**2
if n <=10:
    print(P)
```

    [[ 0.75  0.25  0.    0.    0.    0.    0.    0.    0.  ]
     [ 0.25  0.5   0.25  0.    0.    0.    0.    0.    0.  ]
     [ 0.    0.25  0.5   0.25  0.    0.    0.    0.    0.  ]
     [ 0.    0.    0.25  0.5   0.25  0.    0.    0.    0.  ]
     [ 0.    0.    0.    0.25  0.5   0.25  0.    0.    0.  ]
     [ 0.    0.    0.    0.    0.25  0.5   0.25  0.    0.  ]
     [ 0.    0.    0.    0.    0.    0.25  0.5   0.25  0.  ]
     [ 0.    0.    0.    0.    0.    0.    0.25  0.5   0.25]
     [ 0.    0.    0.    0.    0.    0.    0.    0.25  0.75]]
    


```python
# evals (w) and evecs (v)
w,v = np.linalg.eig(np.transpose(P))
# grab the eigenvector corresponding to the largest eigenvalue (which is 1)
steadystate = v[:,w==np.max(w)]
# but if we want this eigenvector as a probability, we need to normalize it
steadystate = steadystate/steadystate.sum()
print(steadystate)
```

    [[ 0.11111111]
     [ 0.11111111]
     [ 0.11111111]
     [ 0.11111111]
     [ 0.11111111]
     [ 0.11111111]
     [ 0.11111111]
     [ 0.11111111]
     [ 0.11111111]]
    


```python
M = np.zeros([3,3])
p = .5
for i in range(1,2):
    M[i,i] = 2*p*(1-p)
    M[i,i+1] = p**2
    M[i,i-1] = (1-p)**2
M[0,0] = 1-p
M[0,1] = p
M[-1,-1] = p
M[-1,-2] = 1-p
print(M, '\n')

w,v = np.linalg.eig(np.transpose(M))

steadystate = v[:,w==np.max(w)]

steadystate = steadystate/steadystate.sum()
print(steadystate)
```

    [[ 0.5   0.5   0.  ]
     [ 0.25  0.5   0.25]
     [ 0.    0.5   0.5 ]] 
    
    [[ 0.25]
     [ 0.5 ]
     [ 0.25]]
    


```python
T = np.zeros([9,9])
for i in range(1,8):
    T[i,i] = 2*r*(1-p)
    T[i,i+1] = p**2
    T[i,i-1] = (1-p)**2
T[0,0] = 1-p
T[0,1] = p
T[-1,-1] = p
T[-1,-2] = 1-p
print(T, '\n')

w,v = np.linalg.eig(np.transpose(T))

steadystate = v[:,w==np.max(w)]

steadystate = steadystate/steadystate.sum()
print(steadystate)
```

    [[ 0.5   0.5   0.    0.    0.    0.    0.    0.    0.  ]
     [ 0.25  0.5   0.25  0.    0.    0.    0.    0.    0.  ]
     [ 0.    0.25  0.5   0.25  0.    0.    0.    0.    0.  ]
     [ 0.    0.    0.25  0.5   0.25  0.    0.    0.    0.  ]
     [ 0.    0.    0.    0.25  0.5   0.25  0.    0.    0.  ]
     [ 0.    0.    0.    0.    0.25  0.5   0.25  0.    0.  ]
     [ 0.    0.    0.    0.    0.    0.25  0.5   0.25  0.  ]
     [ 0.    0.    0.    0.    0.    0.    0.25  0.5   0.25]
     [ 0.    0.    0.    0.    0.    0.    0.    0.5   0.5 ]] 
    
    [[ 0.0625]
     [ 0.125 ]
     [ 0.125 ]
     [ 0.125 ]
     [ 0.125 ]
     [ 0.125 ]
     [ 0.125 ]
     [ 0.125 ]
     [ 0.0625]]
    
