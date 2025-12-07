The program uses a prime detector (Miller–Rabin) and turns it into a prime computer by counting primes until it reaches the (n)-th one.


1. Small Prime Filtering (Speed-Up Step)
   
Before running the heavier Miller–Rabin test, the code checks divisibility using a list of small primes:
SMALL_PRIMES = [2, 3, 5, 7, ..., 97]

If a number is divisible by one of these, we quickly know it’s composite.
This improves performance by 50–90% because most composite numbers have a small prime factor.

2. The Miller–Rabin Primality Test
This is the core prime detector.
What it does:
For each number n:
Write n-1 = 2^r * d, where d is odd

Repeat the test for t random bases a

Perform fast modular exponentiation to check:

If a^d mod n == 1

Or if repeated squaring ever gives n-1 mod n

If neither happens → n is definitely composite
 If all rounds pass → n is probably prime
The code for this is:
def miller_rabin(n, t=10):
    ...

Why it's used:
Much faster than checking factorials or full division

Error probability decreases exponentially with the number of rounds t

3. The Detector Function (D(k))
This is a simple wrapper over Miller–Rabin:
def D(k, t=10):
    return 1 if miller_rabin(k, t) else 0

It returns:
1 → prime
0 → composite

This is exactly your prime detector → indicator function.

4. Computing the n-th Prime

This implements the formula:
[
 p_n = \min{k : \sum_{i=2}^{k} D(i)=n}
 ]
How it works:
Start from k = 2

For each number:

Check if it's 2 or odd (skips even numbers)

Detect primality using D(k)

Keep a counter of how many primes we have found

When count == n → return k

The code:
def nth_prime(n, t=10):
    count = 0
    k = 2
    while True:
        if k == 2 or k % 2 == 1:
            if D(k, t) == 1:
                count += 1
                if count == n:
                    return k
        k += 1

This is the prime computer.

5. User Input and Output
The program asks the user:
n = int(input("Enter n to compute the nth prime: "))

Then prints the n-th prime:
print(f"The {n}th prime number is: {result}")

1. Time Complexity of  Miller–Rabin Based Method

The algorithm computes the n-th prime by:
Checking all integers from 2 up to the n-th prime

Using the Miller–Rabin primality test as the detector

Breakdown
(a) Number of integers tested
 The n-th prime is approximately:
p_n ≈ n * log(n)

So we test about n log n numbers.

(b) Cost of one Miller–Rabin test
Let:
b = log(n) = number of bits of the numbers being tested

t = number of Miller–Rabin rounds (we use t = 10)

Each Miller–Rabin test takes approximately:
O(t * b^3)
because modular exponentiation is the dominant operation.

(c) Total time complexity
Total work = (numbers tested) × (work per MR test)
T(n) = O( (n log n) * t * (log n)^3 )

Simplified:
Final Complexity:  O( t * n * (log n)^4 )

This is polynomial time and very efficient.

2. Time Complexity of Willans’ Formula
Willans’ formula involves:
An outer summation that goes up to 2^n

An inner expression that contains factorials (j-1)!

The cost of evaluating a factorial-based prime detector is at least proportional to j

Lower bound:
Total work = sum(j from 1 to 2^n) of O(j)
           = O( 2^(2n) )

This is exponential time in n.

