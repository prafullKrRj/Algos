```java
import java.util.*;
class Main
{   
    static void reverseArray(int a[],int n) //This functions reverses the array upto index n
    {
        int temp[]=new int[n+1]; // temp is the temporary array 
        for(int i=0;i<=n;i++)
        {
            temp[i]=a[n-i];
            }
            for(int i=0;i<=n;i++)
            a[i]=temp[i];
        }
       static int findmax(int a[],int n) //Function to find index of max element upto index n
        {
            int max=a[0];int j=0; //j stores the index of max element
            for(int i=1;i<=n;i++)
              if(a[i]>max)
              {max=a[i];
                  j=i;
                }
                return j;
                
            }
    static void pancakesort(int a[],int n)
    {  int currentLength;
        for(currentLength=n;currentLength>0;--currentLength)
        {
            int j= findmax(a,currentLength-1);
            if(j!=currentLength)
            {
                reverseArray(a,j);
                reverseArray(a,currentLength-1);
            }
            }
        }
    public static void main(String[] args)
    {
        Scanner sc=new Scanner(System.in);
        int i,j,k,l,m,n;
        n=sc.nextInt();
        int array[]=new int[n];
        for(i=0;i<n;i++)
        array[i]=sc.nextInt();
        pancakesort(array,n);
        for(i=0;i<n;i++)
        System.out.print(array[i]+" ");
        System.out.println();
    }
}


/* 
Input - 
5 //Array length
4 3 7 1 2 //Array Elements

Output - 
1 2 3 4 7 //Sorted Array
*/
```