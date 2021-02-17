User-defined literals provides a neat way to include physics units
```c++
long double operator "" _nA(long double x) {return x*1e-9}; //base unit for current is 1A
```
which is then called using `2.5_nA`.
