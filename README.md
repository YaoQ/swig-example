# Using SWIG to wrap C/C++ code and call by python

This is copied from [tutorial of SWIG](http://www.swig.org/tutorial.html).
Just for taking a note.

## Steps
### 1. Create an **example.c**

```C
 /* File : example.c */
 
 #include <time.h>
 double My_variable = 3.0;
 
 int fact(int n) {
     if (n <= 1) return 1;
     else return n*fact(n-1);
 }
 
 int my_mod(int x, int y) {
     return (x%y);
 }
    
 char *get_time()
 {
     time_t ltime;
     time(&ltime);
     return ctime(&ltime);
 }
```
### 2. Create an **interface file**
```C
 /* example.i */
 %module example
 %{
 /* Put header files here or function declarations like below */
 extern double My_variable;
 extern int fact(int n);
 extern int my_mod(int x, int y);
 extern char *get_time();
 %}
 
 extern double My_variable;
 extern int fact(int n);
 extern int my_mod(int x, int y);
 extern char *get_time();
```
### 3. Build a python module
```bash
 # swig -python example.i
 # gcc -fPIC -c example.c example_wrap.c \
        -I/usr/local/include/python2.1
# ld -shared example.o example_wrap.o -o _example.so
```
### 4. Use python module
```python
 >>> import example
 >>> example.fact(5)
 120
 >>> example.my_mod(7,3)
 1
 >>> example.get_time()
'Sun Jul 31 09:20:12 2016\n'
 >>>
```
