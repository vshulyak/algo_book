Big O
==============

http://stackoverflow.com/questions/3255/big-o-how-do-you-calculate-approximate-it

Most people with a degree in CS will certainly know what Big O stands for. It helps us to measure how (in)efficient an algorithm really is and if you know in what category the problem you are trying to solve lays in you can figure out if it is still possible to squeeze out that little extra performance.1

But I'm curious, how do you calculate or approximate the complexity of your algorithms?

One
-----------

Big O gives the upper bound for time complexity of an algorithm. It is usually used in conjunction with processing data sets (lists) but can be used elsewhere.

A few examples of how it's used in C code.

Say we have an array of n elements

int array[n];
If we wanted to access the first element of the array this would be O(1) since it doesn't matter how big the array is, it always takes the same constant time to get the first item.

x = array[0];
If we wanted to find a number in the list:

for(int i = 0; i < n; i++){
    if(array[i] == numToFind){ return i; }
}
This would be O(n) since at most we would have to look through the entire list to find our number. The Big-O is still O(n) even though we might find our number the first try and run through the loop once because Big-O describes the upper bound for an algorithm (omega is for lower bound and theta is for tight bound).

When we get to nested loops:

for(int i = 0; i < n; i++){
    for(int j = i; j < n; j++){
        array[j] += 2;
    }
}
This is O(n^2) since for each pass of the outer loop ( O(n) ) we have to go through the entire list again so the n's multiply leaving us with n squared.

This is barely scratching the surface but when you get to analyzing more complex algorithms complex math involving proofs comes into play. Hope this familiarizes you with the basics at least though.

Two
-----------


I'm a professor assistant at my local university on the Data Structures and Algorithms course. I'll do my best to explain it here on simple terms, but be warned that this topic takes my students a couple of months to finally grasp. You can find more information on the Chapter 2 of the Data Structures and Algorithms in Java book.

There is no mechanical procedure that can be used to get the BigOh.

As a "cookbook", to obtain the BigOh from a piece of code you first need to realize that you are creating a math formula to count how many steps of computations gets executed given an input of some size.

The purpose is simple: to compare algorithms from a theoretical point of view, without the need to execute the code. The lesser the number of steps, the faster the algorithm.

For example, let's say you have this piece of code:

int sum(int* data, int N) {
    int result = 0; // 1

    for (int i = 0; i < N; i++) { // 2
        result += data[i]; // 3
    }

    return result; // 4
}
This function returns the sum of all the elements of the array, and we want to create a formula to count the computational complexity of that function:

Number_Of_Steps = f(N)
So we have f(N), a function to count the number of computational steps. The input of the function is the size of the structure to process. It means that this function is called suchs as:

Number_Of_Steps = f(data.length)
The parameter N takes the data.length value. Now we need the actual definition of the function f(). This is done from the source code, in which each interesting line is numbered from 1 to 4.

There are many ways to calculate the BigOh. From this point forward we are going to assume that every sentence that don't depend on the size of the input data takes a constant C number computational steps.

We are going to add the individual number of steps of the function, and neither the local variable declaration nor the return statement depends on the size of the data array.

That means that lines 1 and 4 takes C amount of steps each, and the function is somewhat like this:

f(N) = C + ??? + C
The next part is to define the value of the for statement. Remember that we are counting the number of computational steps, meaning that the body of the for statement gets executed N times. That's the same as adding C, N times:

f(N) = C + (C + C + ... + C) + C = C + N * C + C
There is no mechanical rule to count how many times the body of the for gets executed, you need to count it by looking at what does the code do. To simplify the calculations, we are ignoring the variable initialization, condition and increment parts of the for statement.

To get the actual BigOh we need the Asymptotic analysis of the function. This is roughly done like this:

Take away all the constants C.
From f() get the polynomium in its standard form.
Divide the terms of the polynomium and sort them by the rate of growth.
Keep the one that grows bigger when N approaches infinity.
Our f() has two terms:

f(N) = 2 * C * N ^ 0 + 1 * C * N ^ 1
Taking away all the C constants and redundant parts:

f(N) = 1 + N ^ 1
Since the last term is the one which grows bigger when f() approaches infinity (think on limits) this is the BigOh argument, and the sum() function has a BigOh of:

O(N)
There are a few tricks to solve some tricky ones: use summations whenever you can. There are some handy summation identities already proven to be correct.

As another example, this code can be easily solved using summations:

// A
for (i = 0; i < 2*n; i += 2) { // 1
    for (j=n; j > i; j--) { // 2
        foo(); // 3
    }
}
The first thing you needed to be asked is the order of execution of foo(). While the usual is to be O(1), you need to ask your professors about it. O(1) means (almost, mostly) constant C, independent of the size N.

The for statement on the sentence number one is tricky. While the index ends at 2 * N, the increment is done by two. That means that the for gets executed only N steps, and we need to divide the count by two.

f(N) = Summation(i from 1 to 2 * N / 2)( ... ) = 
     = Summation(i from 1 to N)( ... )
The sentence number two is even trickier since it depends on the value of i. Take a look: the index i takes the values: 0, 2, 4, 6, 8, ..., 2 * N, and the secondth for get executed: N times the first one, N - 2 the secondth, N - 4 the third... up to the N / 2 stage, on which the secondth for never gets executed.

On formula, that means:

f(N) = Summation(i from 1 to N)( Summation(j = ???)(  ) )
Again, we are counting the number of steps. And by definition, every summation should always start at one, and end at a number bigger-or-equal than one.

f(N) = Summation(i from 1 to N)( Summation(j = 1 to (N - (i - 1) * 2)( C ) )
(We are assuming that foo() is O(1) and takes C steps.)

We have a problem here: when i takes the value N / 2 + 1 upwards, the inner Summation ends at a negative number! That's impossible and wrong. We need to split the summation in two, being the pivotal point the moment i takes N / 2 + 1.

f(N) = Summation(i from 1 to N / 2)( Summation(j = 1 to (N - (i - 1) * 2)) * ( C ) ) + Summation(i from 1 to N / 2) * ( C )
Since the pivotal moment i > N / 2, the inner for wont get executed and we are assuming a constant C execution complexity on it's body.

Now the summations can be simplified using some identity rules:

Summation(w from 1 to N)( C ) = N * C
Summation(w from 1 to N)( A (+/-) B ) = Summation(w from 1 to N)( A ) (+/-) Summation(w from 1 to N)( B )
Summation(w from 1 to N)( w * C ) = C * Summation(w from 1 to N)( w ) (C is a constant, independent of w)
Summation(w from 1 to N)( w ) = (w * (w + 1)) / 2
Applying some algebra:

f(N) = Summation(i from 1 to N / 2)( (N - (i - 1) * 2) * ( C ) ) + (N / 2)( C )

f(N) = C * Summation(i from 1 to N / 2)( (N - (i - 1) * 2)) + (N / 2)( C )

f(N) = C * (Summation(i from 1 to N / 2)( N ) - Summation(i from 1 to N / 2)( (i - 1) * 2)) + (N / 2)( C )

f(N) = C * (( N ^ 2 / 2 ) - 2 * Summation(i from 1 to N / 2)( i - 1 )) + (N / 2)( C )

=> Summation(i from 1 to N / 2)( i - 1 ) = Summation(i from 1 to N / 2 - 1)( i )

f(N) = C * (( N ^ 2 / 2 ) - 2 * Summation(i from 1 to N / 2 - 1)( i )) + (N / 2)( C )

f(N) = C * (( N ^ 2 / 2 ) - 2 * ( (N / 2 - 1) * (N / 2 - 1 + 1) / 2) ) + (N / 2)( C )

=> (N / 2 - 1) * (N / 2 - 1 + 1) / 2 = 

   (N / 2 - 1) * (N / 2) / 2 = 

   ((N ^ 2 / 4) - (N / 2)) / 2 = 

   (N ^ 2 / 8) - (N / 4)

f(N) = C * (( N ^ 2 / 2 ) - 2 * ( (N ^ 2 / 8) - (N / 4) )) + (N / 2)( C )

f(N) = C * (( N ^ 2 / 2 ) - ( (N ^ 2 / 4) - (N / 2) )) + (N / 2)( C )

f(N) = C * (( N ^ 2 / 2 ) - (N ^ 2 / 4) + (N / 2)) + (N / 2)( C )

f(N) = C * ( N ^ 2 / 4 ) + C * (N / 2) + C * (N / 2)

f(N) = C * ( N ^ 2 / 4 ) + 2 * C * (N / 2)

f(N) = C * ( N ^ 2 / 4 ) + C * N

f(N) = C * 1/4 * N ^ 2 + C * N
And the BigOh is:

O(N ^ 2)


Three
-------

While knowing how to figure out the Big O time for your particular problem is useful, knowing some general cases can go a long way in helping you make decisions in your algorithm.

Here are some of the most common cases, lifted from http://en.wikipedia.org/wiki/Big_O_notation#Orders_of_common_functions:

O(1) - Determining if a number is even or odd; using a constant-size lookup table or hash table

O(logn) - Finding an item in a sorted array with a binary search

O(n) - Finding an item in an unsorted list; adding two n-digit numbers

O(n^2) - Multiplying two n-digit numbers by a simple algorithm; adding two n×n matrices; bubble sort or insertion sort

O(n^3) - Multiplying two n×n matrices by simple algorithm

O(c^n) - Finding the (exact) solution to the traveling salesman problem using dynamic programming; determining if two logical statements are equivalent using brute force

O(n!) - Solving the traveling salesman problem via brute-force search

O(n^n) - Often used instead of O(n!) to derive simpler formulas for asymptotic complexity

