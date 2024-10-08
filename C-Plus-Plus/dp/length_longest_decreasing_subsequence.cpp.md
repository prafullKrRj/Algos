```cpp
// C++ program to find the Length of Longest Decreasing Subsequence
/* In this problem, given an array we have to find the length of the longest decreasing subsequence that array can make.
The problem can be solved using Dynamic Programming */

#include <bits/stdc++.h>
using namespace std;

int length_longest_decreasing_subsequence(int arr[], int n)
{
    int dp[n], max_len = 0;

    /* Initialize the dp array with the 1 as value, as the maximum length
       at each point is atleast 1, by including that value in the sequence  */
    for (int i = 0; i < n; ++i)
        dp[i] = 1;

    /* Now Lets Fill the dp array in Bottom-Up manner
       Compare Each i'th element to its previous elements from 0 to i-1, 
       If arr[i] < arr[j](where j = 0 to i-1), then it qualifies for decreasing subsequence and
       If dp[i] < dp[j] + 1, then that subsequence  qualifies for being the longest one */
    for (int i = 1; i < n; i++)
    {
        for (int j = 0; j < i; j++)
        {
            if (arr[i] < arr[j] && dp[i] < dp[j] + 1)
                dp[i] = dp[j] + 1;
        }
    }

    //Now Find the maximum element in the 'dp' array
    for (int i = 0; i < n; i++)
    {
        if (dp[i] > max_len)
            max_len = dp[i];
    }

    return max_len;
}

int main()
{
    int n, max_len;
    cout << "\nWhat is the length of the array? ";
    cin >> n;
    int arr[n];
    cout << "Enter the numbers: ";
    for (int i = 0; i < n; i++)
    {
        cin >> arr[i];
    }
    max_len = length_longest_decreasing_subsequence(arr, n);

    cout << "The length of the longest decreasing subsequence of the given array is " << max_len;
    return 0;
}

/*
Time Complexity: O(num ^ 2), where 'num' is the size of the given array
Space Complexity: O(num)

SAMPLE INPUT AND OUTPUT

SAMPLE 1

What is the length of the array? 5
Enter the numbers: 1 2 3 4 3
The length of the longest decreasing subsequence of the given array is 1

SAMPLE 2

What is the length of the array? 8
Enter the numbers: 5 4 58 51 44 5 55 1
The length of the longest decreasing subsequence of the given array is 5
*/

```