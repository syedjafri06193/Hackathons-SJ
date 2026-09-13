
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    long long n;
    cin >> n;
    while (n != 1) {
        cout << n << ' ';
        n = (n % 2 == 0) ? n / 2 : 3 * n + 1;
    }
    cout << 1 << '\n';
}
```

NOTES:
This Collatz Conjecture. I tired to solve this about a year an ago. And I explained geometrically that why it ends up at 1 and what's the logic behind the conjecture. Because the patterns are absurd. However, they kept on putting on useless conditions. Like, let's say that we have to prove that 2+2=4 is true or not. I say √(16) = 4 because (16)^1/2 = 2^2. Or I say 2+2=4 because 1+1+1+1=4. That's how I solved the conjecture. But then it became pointless. It said, such as: no, you have to construct a system for 2+2=4. Let's say I did. Then it said, no, you have to now explain the picture of patterns. So, that's the problem. Even though I solved what I asked, it denied because it kept on increasing conditions for no reason. As a result, I pushed it aside. That's the problem with every problem of mathematics. Whenever you try to solve an age old unsolved mystery, it first says: "Prove 2+2=4" and when you prove it, then it says "No, you also need to prove that why pi^2 = A", pointless.
