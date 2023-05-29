What is Recursion ?
- A function calling itself, is called recursive function.
- pseudo code:
```
Type fun(param) {
	if(<base_condition>)
	{
		1. ------ CALLING TIME - ASCENDING
		2. fun(param) // here it is calling itself.
		3. ------ RETURNING TIME - DESCENDING
	}
}

void main() {
	fun(some_param)
}
```

Example with tracing:
```
void fun2(int n) {
	if(n>0) {
		fun2( n - 1); // (1)
		printf("%d", n); // (2)
	}
}

void main() {
	int x = 3;
	fun2(x);
}

```

<img src="./../resources/recursion/simple_recursion_example_with_tracing.png"  width="100%" height="40%">

LOOP has only Ascending phase but Recursion has Ascending as well as descending phase.


Types of Recursion:
- Tail Recursion
- Head Recursion
- Tree Recursion
- Indirect Recursion
- Nested Recursion
  
#### Tail Recursion: 
- In a Recursive function, when the last statment is calling itself again.
- Only Ascending phase. After the recursive call there is nothing to execute.
- Everything is performed at Calling time only.
Example:
```
// Psceduo code:
Type fun(n){
	if(n>0) {
		------
		------
		------
		fun(n-1); // LAST STATEMENT
	}
}
```

Compare tail recursion with loop:
```
void fun(int n) {
	while(n > 0) {
		printf("%d", n);
		n--;
	}
}

void main() {
	fun(3);
}
```

Tail recursion can be easily converted to Loop. Amount of time spent is same. But the space is more in terms of Recursion.

In case of Recursion: Time: O(n) | Space: O(n).
In case of Loop: Time: O(n) | Space: O(1).

In case of tail recursion the loop is better as space consumption is far less.


#### Head Recursion:
- Function has to do all operation at the time of Returning time but nothing to do at the calling time.
- No statement before the function call.
Example:
```
// Psceduo code:
Type fun(n){
	if(n>0) {
		fun(n-1); // FIRST STATEMENT
		------
		------
		------
	}
}
```

```
void fun(int n) {
	if(n>0){
		fun(n-1);
		printf("%d ", n);
	}
}
void main(){
	fun(3);
}

o/p: 1 2 3
```
It can be converted in terms of loop but it is not as easy and straighforward as it was for tail recursion.

```
void fun(int n) {
	int i = 1;
	while(i<=n){
		printf("%d ", i);
		i++;
	}
}
void main(){
	fun(3);
}

o/p: 1 2 3
```

#### Tree Recursion:
- If recursive function calling itself more than one-time.

```
fun(n) {
	if(n>0) {
		----
		----
		fun(n-1);
		----
		----
		fun(n-1);
		----
		----
		----
	}
}
```


```
void fun(int n) {
	if(n>0) {
		printf("%d ", n);
		fun(n-1);
		fun(n-1);
	}
}

void main(){
	fun(3);
}
```



#### Linear Recursion:
- If recursive function calling only one-time.
```
fun(n) {
	if(n>0) {
		----
		----
		fun(n-1);
		----
		----
		----
	}
}
```


#### Indirect Recursion:
Example:
```
void funA(int n) {
    if(n>0) {
        printf("%d", n);
        funB(n-1);
    }
}

void funB(int n) {
    if(n>0) {
        printf("%d", n);
        funa(n/2);
    }
}

void main() {
    funA(20);
}
```

#### Nested Recursion:
- Recursive call taking recursive call as a parameter.

```
void fun(int n) {
	if(----) {

		----;
		----;
		fun(fun(n-1));
	}
}
```


Example:
```
void fun(int n) {
	if(n>100) {
		return n - 10;
	} else {
		return fun(fun(n+11));
	} 
}

void main() {
	fun(95);
}
```



##### Power using recursion:

```
int pow(int m, int n) {
	if(n == 0)
		return 1;

	if(n%2 ==0 ){
		return pow(m*m ,n/2);
	} else {
		return m*pow(m*m ,(n-1)/2);
	}
}

void main(){
	pow(2, 9);
}
```

##### Taylor series using recursion:

<p><i>e</i><sup>x</sup> = 1 + x + <span><em>x<sup>2</sup></em><strong>/2!</strong></span> + <span ><em>x<sup>3</sup></em><strong>/3!</strong></span> + <span><em>x<sup>4</sup></em><strong>/4!</strong></span> + <span ><em>x<sup>5</sup></em><strong>/5!</strong></span> + ... + n terms</p>

```
double e(int x , int n) {

	static double p, f = 1;
	double res;

	if(n == 0) {
		return 1;
	} else {
		r = e(x, n-1);
		p = p * x;
		f = f * n;
		return r + p/f;
	}
}

void main() {
e(1, 10);	
}
```
##### Taylor series using Horners Rule:
https://dotnettutorials.net/lesson/taylor-series-using-horners-rule/
e.g.
```
double e (int x, int n) {
    static double s;
    if (n == 0)
        return s;
    s = 1 + ((x/n) * s) ;
    return e (x, n - 1);
}
```

##### Fibonacci using Iteration:
```
int fib(int n) {

int firstNum = 0, secondNum=1,s,i;

if(n<=1) return n;

for (i=2; i<=n; i++) {
	
}



}
```