```py
"""
Purpose: Total number of Unique BST's that can be
         made using n keys/nodes.
Method: Dynamic Programming
Intution: Total number of Unique BST's with N nodes
            = Catalan(N)

Here function Unique_BST() return the total number of
diffrent Binary Search Trees that can be made with N distinct nodes

Argument: N (number of distinct nodes)
return Type: int (Total number of binary tree)

Time Complexity:  O(n)
Space Complexity: O(n)

Note: Since the number of possible binary search tree will be large
      the answer is given in mod of 10^9+7
      
"""

# Catalan_Number (N) = ((2*N)!) / ((N+1)!*N!)

# Global Variables
MOD = 10**9+7
facto = [1]  # Factorial Table

# To construct the factorial numbers using DP


def factorial(n):
    global facto
    for i in range(1, n+1):
        facto += [(facto[-1]*i) % MOD]


# For Modular Inverse of num with respect to 10^9+7
def Mod_Inv(num):
    return pow(num, MOD-2, MOD)


def Catalan_Number(num):
    if num == 0 or num == 1:
        return 1

    # Constructing Factorial Table
    factorial(2*num)
    Numerator = facto[2*num]
    Denominator = (facto[num+1]*facto[num]) % MOD

    Catalan = (Numerator * Mod_Inv(Denominator)) % MOD
    return Catalan


def Unique_BST(N):
    
    # Nth Catalan Number
    Cat = Catalan_Number(N)
    return Cat

# ------------------------DRIVER CODE ------------------------

if __name__ == "__main__":
    n = int(input("Enter the number of distinct nodes: "))
    print("Total number of Binary Search Tree = ", Unique_BST(n))

"""
SAMPLE INPUT/OUTPUT

Enter the number of distinct nodes: 5
Total number of Binary  Search Tree =  42

Enter the number of distinct nodes: 10
Total number of Binary Search Tree =  16796

"""

```