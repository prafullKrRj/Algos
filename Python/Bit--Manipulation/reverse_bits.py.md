```py
"""
Python program to reverse the bits of a number
Given an integer, reverse its bits in its binary equivalent and
print the new number obtained in its decimal form
"""


def reverse_bits(n):
    rev = 0

    while(n > 0):
        # Shift the bit of the reversed(answer) number to the right
        rev = rev << 1
        #  Stores the temporary lsb of the given number
        rem = n & 1
        # Set the lsb of the answer variable with the stored value
        rev = rem | rev
        # Drops the already processed lsb of the given number
        n = n >> 1
    return rev


if __name__ == '__main__':
    print("Enter the number? ", end="")
    num = int(input())
    rev = reverse_bits(num)
    print("The bits-reversed number is: {}".format(rev))

"""
Time Complexity: O(n)
Space Complexity: O(1)

SAMPLE INPUT AND OUTPUT

Enter the number? 39
The bits-reversed number is: 57
"""

```