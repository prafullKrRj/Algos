```cpp
// Minimum Sum Partition using recursion
#include <iostream>
#include <string>
using namespace std;

/* Partition the Set into two subsets Set1, Set2 such that the
  difference between the sum of elements in Set1 and the sum
  of elements in Set2 is minimized */
int minPartition(int Set[], int n, int Set1, int Set2)
{
    /* Case 0. If list becomes empty, return the absolute
       difference between two sets */
    if (n < 0)
        return abs(Set1 - Set2);

    /* Case 1. Include current item in the subset Se1 and recur
       for remaining items (n - 1) */
    int inc = minPartition(Set, n - 1, Set1 + Set[n], Set2);

    /* Case 2. Exclude current item from subset Se1 and recur for
       remaining items (n - 1) */
    int exc = minPartition(Set, n - 1, Set1, Set2 + Set[n]);

    //Returning included and excluded values
    return min(inc, exc);
}

//Main Code
int main()
{

    //int Set[] = { 10, 20, 15, 5, 25};

    int size;
    std::cout << "Enter the number of elements you want in Set: ";
    std::cin >> size;
    int Set[size]; //Creating Set of size 'size'
    std::cout << "Enter elements: " << std::endl;

    for (int i = 0; i < size; i++) //Enter the elements in the Set
    {
        std::cin >> Set[i];
    }
    // Number of elements
    int n = sizeof(Set) / sizeof(Set[0]);

    cout << "\nThe entered elements in the Set are: \n" << std::endl;
    for (int i = 0; i < size; ++i) {
        cout << Set[i] << " ";
    }
    //Printing the minimum difference
    cout << "\nThe minimum difference is " << minPartition(Set, n - 1, 0, 0);

    return 0;
}

/*
SAMPLE INPUT:
5
1
2
3
4
5
SAMPLE OUTPUT:
Enter the number of elements you want in Set: Enter elements: 

The entered elements in the Set are: 

1 2 3 4 5 
The minimum difference is 1

Time Complexity = O(n*sum) 
Depends on the value of 'n' and 'sum'
*/

```