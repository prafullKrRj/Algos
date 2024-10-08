```cpp
/*

Introduction 
Given a Binary Tree , invert it and print its Levelorder

Argument/Return Type
Input of total no.of nodes is taken
Input of key values of nodes of tree are taken in level order form 
Incase of a null node , -1 is taken as input
Level Order of inverted tree is printed as output

*/

//  Code / Solution

#include <bits/stdc++.h> 
using namespace std;

//Define Node as structure
struct Node 
{
    int key;
    Node* left;
    Node* right;
};
 
//Function to create a node with 'value' as the data stored in it. 
//Both the children of this new Node are initially null.
struct Node* newNode(int value)
{
    Node* n = new Node;
    n->key = value;
    n->left = NULL;
    n->right = NULL;
    return n;
}

//Function to build tree with given input
struct Node* createTree(vector<int>v)
{
    int n=v.size();
    if(n==0) 
      return NULL;
    vector<struct Node* >a(n);
    //Create a vector of individual nodes with given node values
    for(int i=0;i<n;i++)  
    {
        //If the data is -1 , create a null node
        if(v[i]==-1)  
          a[i] = NULL;
        else 
          a[i] = newNode(v[i]);
    }
    //Interlink all created nodes to create a tree
    //Use two pointers using int to store indexes
    //One to keep track of parent node and one for children nodes
    for(int i=0,j=1;j<n;i++) 
    {
        //If the parent node is NULL , advance children pointer twice
        if(!a[i])
        {
          j=j+2;
          continue;
        } 
        //Connect the two children nodes to parent node
        //First left and then right nodes
        a[i]->left = a[j++];
        if(j<n) 
          a[i]->right = a[j++];
    }
    return a[0];
}

//Function to print Level order traversal of given tree
void levelOrder(struct Node* root)
{
    //If root is NULL , return it
    if (root == NULL)
        return;
    //Create a queue
    queue<Node*> q;
    //Push the root to the queue
    q.push(root);
    while (!q.empty()) 
    {
        //For each node we visit ,
        //Print its value , push their children if they exist
        //Pop it . Repeat this process till queue becomes empty
        cout << q.front()->key << " ";
        if (q.front()->left != NULL)
            q.push(q.front()->left);
        if (q.front()->right != NULL)
            q.push(q.front()->right);
        q.pop();
    }
} 

//Invert the given tree
void Invert(struct Node* node) 
{
    //If the node is NULL , return it 
    if (node == NULL) 
        return; 
    else
    {
        struct Node* temp;
          
        // recursively invert the subtrees
        Invert(node->left);
        Invert(node->right);
      
        // swap the pointers in this node */
        swap(node->left,node->right);
    }
} 

//Driver code
int main()
{

    int n;
    cout<<"Enter total no.of nodes of the input Tree ( including NULL nodes ) : ";
    cin>>n;

    vector<int>v(n);
    cout<<"Enter value of each node of the tree in level order ( if a node is NULL , enter -1 ) with spaces"<<endl;
    for(int i=0;i<n;i++)
    {
        cin>>v[i]; //store the input values in a vector
    }

    //create the tree using input node values  
    struct Node* root=createTree(v); 

    Invert(root); 

    // Print Level order and inorder traversals of inverted tree
    cout << "Level order traversal of the inverted tree is : ";
    levelOrder(root);
      
    return 0; 
}
    
/*
Input:
0 <= node->key < 1000000000
if node is NULL , -1 is entered as it's key

Sample Test Case  
Input Binary Tree :                    Output Inverted Binary Tree:

               1                                     1
           /         \                          /         \
         2              3                     3              2
     /       \        /   \               /       \        /   \
    4       NULL     5     6             6         5      NULL   4
   / \      /  \    / \   /  \          /  \      /  \    / \   /  \       
  7 NULL  NULL NULL 8  9 10  NULL      NULL 10 9 8 NULL NULL NULL  7    


Input Format : 
Example :
Enter total no.of nodes of the input Tree ( including NULL nodes ) : 15
Enter value of each node of the tree in level order ( if a node is NULL , enter -1 ) with spaces
1 2 3 4 -1 5 6 7 -1 -1 -1 8 9 10 -1

Output Format :
Example : ( Output to the above input example ) 
Level order traversal of the inverted tree is : 1 3 2 6 5 4 10 9 8 7 

Time/Space Complexity
Time Complexity : O(n) 
   Where n is the no.of nodes 
Space Complexity : O(n) 
   Where n is the no.of nodes
*/

```