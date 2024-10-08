```js
/*
The traversal operation is a frequently used operation on a binary tree. This operation is used to visit each node in the tree exactly once
A full traversal on a binary tree gives a linear ordering of data in the tree .  This is the iterative preorder tree traversal algorithms
*/

class Stack {
    constructor() {
        this.items = [];
    }
    push(element) {
        this.items.push(element)
    }
    pop() {
        if (this.items.length == 0) {
            return "Underflow";
        }
        return this.items.pop();
    }
    peek() {
        return this.items[this.items.length - 1]
    }
    isEmpty() {
        return this.items.length == 0;
    }
}




class Node {
    constructor(data) {
        this.data = data;
        this.left = null;
        this.right = null;
    }
}

class BinarySearchTree {
    constructor() {
        this.root = null;
    }
    //insert function inserts a new node into the binary search tree
    insert(value) {
        var New = new Node(value);
        if (this.root === null) {
            this.root = New;
            return this;
        }
        let curr = this.root;
        let prev = null;
        while (curr) {
            if (value < curr.data) {
                prev = curr;
                curr = curr.left;
            }
            else if (value > curr.data) {
                prev = curr;
                curr = curr.right;
            }
        }
        if (prev.data > value) {
            prev.left = New;
            return this;
        }
        else {
            prev.right = New;
            return this;
        }

    }

    iterative_preorder(node) {
        let ptr = node;
        let stack = new Stack();
        let s = ""
        stack.push(ptr);
        while (!stack.isEmpty()) {
            ptr = stack.pop();
            if (ptr != null) {
                s += ptr.data + " "
                stack.push(ptr.right)
                stack.push(ptr.left)
            }
        }
        console.log(s)
    }
}

const readline = require('readline');
const rl = readline.createInterface({ input: process.stdin, output: process.stdout });

const getLine = (function () {
    const getLineGen = (async function* () {
        for await (const line of rl) {
            yield line;
        }
    })();
    return async () => ((await getLineGen.next()).value);
})();

const main = async () => {
    console.log("Enter the number of nodes to insert");
    let n = Number(await getLine());
    let b = new BinarySearchTree();
    console.log("Enter the values of nodes to insert\n")
    for (let i = 0; i < n; i++) {
        console.log("Enter the value for node " + (i + 1));
        b.insert(Number(await getLine()));
    }
    let root = b.root;
    console.log("The preorder traversal for the binary search tree ")
    b.iterative_preorder(root);
    process.exit(0);
}

main();

/*
Sample I/O:
Enter the number of nodes to insert
5
Enter the values of nodes to insert

Enter the value for node 1
3
Enter the value for node 2
4
Enter the value for node 3
2
Enter the value for node 4
5
Enter the value for node 5
1
The preorder traversal for the binary search tree
3 2 1 4 5

Time Complexity = O( n )
Space Complexity = O( n )
*/

```